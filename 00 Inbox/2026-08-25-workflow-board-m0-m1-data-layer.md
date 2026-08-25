# Workflow Board M0–M1: Build System and Data Layer — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Turn `Claude_Workflow_V25.jsx` into a buildable, reviewable Vite application, then extract a normalised, unit-tested board data layer that can accept per-cell last-write-wins ops and replay an op log — without changing any user-visible behaviour.

**Architecture:** A Vite + React + Tailwind project wraps the existing single-file component unchanged. A `window.storage` shim backed by `localStorage` replaces the Claude sandbox API so the app runs in an ordinary browser. Then a pure-function store (`src/store/`) normalises the legacy nested board blob into flat entity maps keyed by id, exposes selectors that reproduce the exact shapes the existing render code already consumes, and applies ops with server-authoritative LWW resolution. The store is proven by a legacy round-trip property test before any consumer is switched to it.

**Tech Stack:** Vite 5, React 18, Tailwind CSS 3.4, Zustand 4, Vitest 2, Prettier 3, `lucide-react`, `mammoth`. All packages from JFrog Artifactory.

**Spec:** [`docs/superpowers/specs/2026-08-25-workflow-board-cats-migration-design.md`](../specs/2026-08-25-workflow-board-cats-migration-design.md)

**Milestones covered:** M0 (Prettier reformat) and M1 (store and selectors, test-first, still on `window.storage`). M2–M5 are separate plans.

## Global Constraints

- **Packages come only from JFrog Artifactory.** npm registry `https://elilillyco.jfrog.io/artifactory/api/npm/Lilly-NPM/`. Direct npmjs.org is prohibited (organisation mandate). Anonymous pull was disabled 2026-05-04, so every install authenticates.
- **No behaviour change in M0–M1.** The app must look and act identically at every commit. This plan adds a build system and a data layer; it does not add features or alter the UI.
- **Test-first for every pure function** in `src/store/` and `src/sync/`. Write the failing test, watch it fail, then implement.
- **No temp files, no disk writes** anywhere in code that will later run in a pod — `readOnlyRootFilesystem: true` is mandatory on CATS. (Relevant here only as a habit; M0–M1 is browser-side.)
- **Ordering is server-authoritative.** `clientTs` is recorded for diagnostics and never used for ordering. Ordering is `(serverTs, seq)` lexicographic.
- **Ids are monotonic integers**, produced by the existing `uid()` helper. Do not switch to UUIDs; the legacy data and its round-trip depend on integer ids.
- **Item name is not a cell.** `Name` is a rendered column but its value lives on the item record, never in `cells`.

## Prerequisites (do these before Task 1)

1. **Artifactory npm auth.** Already configured on this machine — `~/.npmrc` holds a `_auth` credential for `//elilillyco.jfrog.io/artifactory/api/npm/Lilly-NPM/`, and it works. Confirm before starting:
   ```bash
   npm whoami && npm view react@18.3.1 version
   ```
   Expected: your Lilly address, then `18.3.1`. If instead you get `401 Unauthorized`, the stored credential has expired — refresh it in `~/.npmrc`. Do not switch registries.
2. **Node 20+.** Verified present: `node v24.13.0`, `npm 11.6.2`.

`npm` also prints `warn Unknown user config "email"` on every command, from a deprecated `email=` line in `~/.npmrc`. It is harmless and unrelated to this work — ignore it rather than treating it as a failure signal.

## Deviations from the spec, and why

Reading the source turned up three things §5 of the spec does not account for. Each is recorded here rather than silently absorbed.

**1. Headers carry field values, so `cells` cannot be keyed on rows alone.**

The spec models `headers (id, category_id, name, position, ...)` and `rows (...)` as different things, with `cells (row_id, column_id, ...)`. But in the source a header and a row have an identical field set:

```js
// header  (line 285)  {id, name, priority, dueDate, status, isExpanded, custom, subtasks: []}
// row     (line 286)  {id, name, priority, dueDate, status, custom}
```

and the single value-getter `gV(it, col)` is applied to both. A header is a row that has children. If `cells` were keyed on rows only, every header's Priority, Due Date, Status, and custom values would have nowhere to live.

**Resolution:** one `items` table/map with `kind: 'header' | 'row'` and a nullable `parentItemId`, and `cells` keyed on `itemId`. This is strictly more faithful to the data and collapses two entities into one. **§5 of the spec should be amended to match before the M2 plan is written** — flagged for the author.

**2. `cc` and `cols` are different things, and only one of them is per-user.**

`migCC` settles it:

```js
const migCC=(cc,cols)=>{const c=[...(cc||[])];["Priority","Due Date","Status"].forEach(d=>{if((cols||[]).includes(d)&&!c.includes(d))c.push(d)});return c};
```

`cc` is the column *registry* (every column that exists on the board); `cols` is the shared, ordered display list; `visCols` is the per-user show/hide layer on top. `stripViewState` deletes `visCols` but keeps `cc` and `cols`, confirming the split. A column entity therefore needs **two** orderings: `position` (index in `cc`) and `displayIndex` (index in `cols`, or `null`).

**3. `ops.kind` is missing `row.rename`.**

§5 enumerates `header.rename` but no `row.rename`. A row's name is editable in the UI exactly as a header's is, so without it there is no op that can express the edit. Added to the closed set in Task 8.

**4. `archive` is board data and is not modelled.**

Each board carries an `archive` array of previously-deleted items bearing `_archivedAt`, `_fromCategory`, `_fromCategoryId`. It is not in `stripViewState`, so it is shared state. Normalising it into `items` is **out of scope for M0–M1**: it is carried verbatim as an opaque field on the board record so it round-trips losslessly. The M2 plan models it properly.

## File structure

```
.npmrc                             Artifactory registry pin only — no credentials (see Task 1 Step 1)
.prettierrc                        Formatting config (M0)
.gitignore                         node_modules, dist, .env, .claude/worktrees
package.json                       deps + scripts
vite.config.js                     React plugin, Vitest config
tailwind.config.js                 content globs
postcss.config.js                  tailwind + autoprefixer
index.html                         mount point
src/main.jsx                       React root, imports the storage shim first
src/index.css                      Tailwind directives
src/App.jsx                        the existing Artifact, moved (unchanged in Task 1)
src/platform/storage-shim.js       window.storage over localStorage; deleted at M2
src/store/shape.js                 createEmptyState, entity field lists, comparators
src/store/normalize.js             normalizeBoard  (legacy blob -> flat entities)
src/store/denormalize.js           denormalizeBoard (flat entities -> legacy blob)
src/store/selectors.js             selectors returning legacy-shaped data
src/store/ops.js                   op constructors + applyOp + applyOps
src/store/index.js                 Zustand store, persistence to window.storage
src/sync/queue.js                  offline op queue
tests/store/*.test.js              one file per store module
tests/fixtures/legacy-board.js     hand-built legacy board covering every field
```

Each store module is a pure function module with no React and no I/O, so it is testable in isolation. `src/store/index.js` is the only file that touches Zustand or storage.

---

### Task 1: Scaffold a buildable Vite app around the unchanged Artifact

The Artifact cannot currently be built or run at all — there is no `package.json`, and it depends on three things the sandbox provided ambiently: the `window.storage` API, Tailwind's stylesheet, and bare-specifier imports. This task makes it run in a browser with **zero edits to the component**, so that the Prettier commit in Task 2 has a build to be verified against.

**Files:**
- Create: `.npmrc`, `.gitignore`, `package.json`, `vite.config.js`, `tailwind.config.js`, `postcss.config.js`, `index.html`, `src/main.jsx`, `src/index.css`, `src/platform/storage-shim.js`
- Move: `Claude_Workflow_V25.jsx` → `src/App.jsx` (content unchanged)

**Interfaces:**
- Consumes: nothing.
- Produces: a working `npm run dev` and `npm run build`; `window.storage` with the same contract the Artifact expects — `get(key, shared?) -> Promise<{value: string} | null>` and `set(key, value, shared?) -> Promise<void>`.

- [ ] **Step 1: Point npm at Artifactory**

Create `.npmrc` with the registry and **no credential line**:

```ini
registry=https://elilillyco.jfrog.io/artifactory/api/npm/Lilly-NPM/
```

Pinning the registry in the repo makes the Artifactory requirement travel with the code, so a fresh clone or a CI runner cannot silently fall back to npmjs.org. Credentials deliberately stay out: locally they resolve from `~/.npmrc`, and in CI the `EliLillyCo/.github/actions/artifactory-login@main` step writes its own auth via OIDC.

Do **not** add a `:_authToken=${NPM_AUTH_TOKEN}` line here. A project-level auth entry takes precedence over the user-level one for the same registry, so with `NPM_AUTH_TOKEN` unset it would replace working credentials with an empty token and turn a functioning install into a 401.

Create `.gitignore`:

```gitignore
node_modules/
dist/
.env
.env.local
.claude/worktrees/
```

`.claude/worktrees/` is where the harness places isolated worktrees — inside the working tree, so without this line a worktree's entire contents show up as untracked files in the parent checkout and can be committed by a stray `git add .`. Ignore that path only, not all of `.claude/`, so a settings file committed later still works.

- [ ] **Step 2: Create package.json**

```json
{
  "name": "workflow-board",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "test": "vitest run",
    "test:watch": "vitest",
    "format": "prettier --write \"src/**/*.{js,jsx,css}\" \"tests/**/*.js\"",
    "format:check": "prettier --check \"src/**/*.{js,jsx,css}\" \"tests/**/*.js\""
  },
  "dependencies": {
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "lucide-react": "^0.445.0",
    "mammoth": "^1.8.0",
    "zustand": "^4.5.5"
  },
  "devDependencies": {
    "@vitejs/plugin-react": "^4.3.1",
    "autoprefixer": "^10.4.20",
    "postcss": "^8.4.47",
    "prettier": "^3.3.3",
    "tailwindcss": "^3.4.13",
    "vite": "^5.4.8",
    "vitest": "^2.1.1"
  }
}
```

Tailwind is pinned to 3.x deliberately: every utility class in the Artifact was written against v3, and v4 renames and removes some of them.

- [ ] **Step 3: Install and verify Artifactory auth works**

Run: `npm install`
Expected: completes and creates `node_modules/`. Then confirm every package actually came from Artifactory and nothing leaked to the public registry:

```bash
grep -c "registry.npmjs.org" package-lock.json
```

Expected: `0`. A non-zero count means something bypassed the registry setting — fix that before continuing rather than proceeding with mixed provenance. A `401 Unauthorized` means the stored credential expired; refresh `~/.npmrc`, do not switch registries.

- [ ] **Step 4: Create the build config**

`vite.config.js`:

```js
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()],
  server: { port: 5173 },
  test: {
    environment: "node",
    include: ["tests/**/*.test.js"],
  },
});
```

`tailwind.config.js` — the content glob must cover `src/App.jsx`, which is where every utility class lives:

```js
export default {
  content: ["./index.html", "./src/**/*.{js,jsx}"],
  theme: { extend: {} },
  plugins: [],
};
```

No `safelist` is needed: every Tailwind class in the Artifact is a string literal (verified — zero template-literal or interpolated class names), so the scanner finds them all.

`postcss.config.js`:

```js
export default {
  plugins: { tailwindcss: {}, autoprefixer: {} },
};
```

`src/index.css`:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

- [ ] **Step 5: Write the storage shim**

The Artifact calls `window.storage.get(key, shared)` and `window.storage.set(key, value, shared)`, and expects `get` to resolve to `{value: string}` or a falsy value. It uses exactly five key families: `task-tracker-v7` (private), and `sb:<id>`, `sb-idx`, `owned-boards`, `joined-codes`, `sb-preflight` (the last five carry `shared: true` for board sharing).

This shim keeps the app runnable locally and is **deleted at M2**, when a real backend replaces it. `shared` is honoured only as a key prefix — one browser cannot really share with another, so sharing appears to work but is single-user. That is the correct M1 behaviour: the spec puts real sharing in M2.

`src/platform/storage-shim.js`:

```js
// Stands in for the Claude Artifact sandbox's window.storage API so the app runs in an
// ordinary browser. Deleted at M2 when the FastAPI backend takes over.
// Contract, matching what App.jsx already calls:
//   get(key, shared?) -> Promise<{value: string} | null>
//   set(key, value, shared?) -> Promise<void>

const prefix = (shared) => (shared ? "shared:" : "private:");

export function installStorageShim(target = window) {
  if (target.storage && typeof target.storage.get === "function") return;
  target.storage = {
    async get(key, shared = false) {
      const raw = localStorage.getItem(prefix(shared) + key);
      return raw === null ? null : { value: raw };
    },
    async set(key, value, shared = false) {
      localStorage.setItem(prefix(shared) + key, String(value));
    },
  };
}
```

- [ ] **Step 6: Create the entry point and move the Artifact**

`index.html`:

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Workflow Board</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```

`src/main.jsx` — the shim must be installed **before** `App` is imported, because the component reads `window.storage` during mount:

```jsx
import { installStorageShim } from "./platform/storage-shim.js";

installStorageShim();

const { createRoot } = await import("react-dom/client");
const { default: App } = await import("./App.jsx");
import("./index.css");

createRoot(document.getElementById("root")).render(<App />);
```

Move the Artifact without editing it:

```bash
git mv Claude_Workflow_V25.jsx src/App.jsx
```

- [ ] **Step 7: Verify the app builds**

Run: `npm run build`
Expected: succeeds, writes `dist/`. Any error here is a real problem with the Artifact's imports or JSX — fix the config, not the component.

- [ ] **Step 8: Verify the app runs and is styled**

Run: `npm run dev`, then open `http://localhost:5173`.
Expected: the board UI renders **with styling** (coloured priority dots, grey borders, correct fonts). Create a category, a header, and a row; reload the page; they persist. Unstyled output means the Tailwind content glob is wrong; a blank page with a console error about `window.storage` means the shim import order in `main.jsx` is wrong.

- [ ] **Step 9: Commit**

```bash
git add -A
git commit -m "build: scaffold Vite app around the Artifact, unchanged

Adds package.json, Vite/Tailwind/PostCSS config, an entry point, and a
localStorage-backed window.storage shim so the component runs outside the
Claude sandbox. src/App.jsx is byte-identical to Claude_Workflow_V25.jsx.

Co-Authored-By: Lilly Code <lillycode@lilly.com>"
```

---

### Task 2: Prettier-only reformat (M0)

The longest line in `App.jsx` is 12,209 characters; four others exceed 3,800. No diff against that is reviewable, and hand-editing it is guesswork. This commit changes formatting and nothing else, so that every subsequent diff in this plan and the next is readable.

**Files:**
- Create: `.prettierrc`
- Modify: `src/App.jsx` (formatting only)

**Interfaces:**
- Consumes: the working build from Task 1.
- Produces: an `App.jsx` whose diffs are reviewable. No API change.

- [ ] **Step 1: Record the pre-format build output hash**

The only meaningful check that a reformat changed no behaviour is that the bundle is semantically identical. Capture a baseline:

```bash
npm run build && node -e "const{readdirSync,readFileSync}=require('fs');const{createHash}=require('crypto');const d='dist/assets';const js=readdirSync(d).filter(f=>f.endsWith('.js')).sort();console.log(js.map(f=>createHash('sha256').update(readFileSync(d+'/'+f)).digest('hex').slice(0,16)+' '+f).join('\n'))"
```

Record the output. Note the filename contains a content hash, so **the names are expected to change** if anything at all differs — this baseline is for comparing *behaviour manually*, not for asserting byte equality. Minified output is not guaranteed identical across a reformat.

- [ ] **Step 2: Create the Prettier config**

`.prettierrc`:

```json
{
  "printWidth": 100,
  "tabWidth": 2,
  "semi": true,
  "singleQuote": false,
  "trailingComma": "es5",
  "bracketSpacing": true,
  "arrowParens": "always",
  "endOfLine": "lf"
}
```

- [ ] **Step 3: Reformat**

Run: `npm run format`
Expected: `src/App.jsx` is rewritten. Confirm the longest line is now bounded:

```bash
awk '{ print length }' src/App.jsx | sort -rn | head -3
```

Expected: all three values well under 200. (Prettier cannot break some long string literals, so a handful of lines may exceed `printWidth` — that is fine. A line still in the thousands means Prettier failed to parse and silently skipped the file; check its exit code.)

- [ ] **Step 4: Verify the build still succeeds**

Run: `npm run build`
Expected: succeeds with no new warnings.

- [ ] **Step 5: Verify behaviour by hand**

Run: `npm run dev`. Exercise, at minimum: add a category, add a header under it, add a row under the header, set Priority and Due Date and Status on both the header and the row, collapse and expand the category, apply a filter, sort by a column, switch to calendar view, switch to Gantt view, reload and confirm everything persisted.
Expected: identical to Task 1 Step 8. A reformat that breaks something has broken a template literal or an ASI-dependent line — `git diff` the region and fix it manually.

- [ ] **Step 6: Commit**

```bash
git add .prettierrc src/App.jsx
git commit -m "style: prettier-only reformat of App.jsx (M0)

No behaviour change. The longest line was 12,209 characters, which made
every diff unreviewable and every edit guesswork. Verified by build plus
manual exercise of categories, headers, rows, filters, sort, calendar,
and gantt.

Co-Authored-By: Lilly Code <lillycode@lilly.com>"
```

---

### Task 3: Normalised state shape and ordering comparator

The foundation every later task builds on. Two pieces: the empty-state factory, and the `(serverTs, seq)` comparator that decides which of two writes to a cell wins. The comparator is small and load-bearing enough to deserve its own tests — getting it wrong is a silent data-loss bug.

**Files:**
- Create: `src/store/shape.js`
- Test: `tests/store/shape.test.js`

**Interfaces:**
- Consumes: nothing.
- Produces:
  - `createEmptyState() -> State`
  - `BUILTIN_FIELD_BY_KEY: {"Priority": "priority", "Due Date": "dueDate", "Status": "status"}`
  - `isNewer(candidate, existing) -> boolean` where both are `{updatedAt: number, seq: number}`

`State` is:

```js
{
  boards:     { [boardId]: {id, name, color, folderId, archive, nameWidth,
                            categoryIds: [], columnIds: [],
                            updatedAt, updatedBy, isDeleted} },
  categories: { [id]: {id, boardId, name, position, updatedAt, updatedBy, isDeleted} },
  items:      { [id]: {id, boardId, categoryId, parentItemId, kind, name, position,
                       updatedAt, updatedBy, isDeleted} },
  columns:    { [id]: {id, boardId, key, label, type, options, color, width,
                       position, displayIndex, updatedAt, updatedBy, isDeleted} },
  cells:      { [itemId]: { [columnId]: {value, updatedAt, updatedBy, seq} } },
  view:       { [boardId]: {gFilters, cFl, crFl, srtRules, visCols,
                            calDotCol, calBorderCol, expanded} },
  seq:        { [boardId]: number },
}
```

`kind` is `'header' | 'row'`. `parentItemId` is `null` for headers and the header's id for rows. `position` on a column is its index in `cc`; `displayIndex` is its index in `cols`, or `null` when the column is not displayed. `expanded` is `{[categoryOrItemId]: boolean}`.

- [ ] **Step 1: Write the failing tests**

`tests/store/shape.test.js`:

```js
import { describe, it, expect } from "vitest";
import { createEmptyState, isNewer, BUILTIN_FIELD_BY_KEY } from "../../src/store/shape.js";

describe("createEmptyState", () => {
  it("returns every entity map empty", () => {
    const s = createEmptyState();
    expect(s).toEqual({
      boards: {},
      categories: {},
      items: {},
      columns: {},
      cells: {},
      view: {},
      seq: {},
    });
  });

  it("returns a fresh object each call", () => {
    const a = createEmptyState();
    a.boards.x = 1;
    expect(createEmptyState().boards).toEqual({});
  });
});

describe("isNewer", () => {
  it("is true when the candidate timestamp is greater", () => {
    expect(isNewer({ updatedAt: 200, seq: 1 }, { updatedAt: 100, seq: 9 })).toBe(true);
  });

  it("is false when the candidate timestamp is smaller", () => {
    expect(isNewer({ updatedAt: 100, seq: 9 }, { updatedAt: 200, seq: 1 })).toBe(false);
  });

  it("breaks equal timestamps by seq, matching the SQL guard", () => {
    expect(isNewer({ updatedAt: 100, seq: 5 }, { updatedAt: 100, seq: 4 })).toBe(true);
    expect(isNewer({ updatedAt: 100, seq: 4 }, { updatedAt: 100, seq: 5 })).toBe(false);
  });

  it("rejects a re-delivery of the identical op, making apply idempotent", () => {
    expect(isNewer({ updatedAt: 100, seq: 5 }, { updatedAt: 100, seq: 5 })).toBe(false);
  });

  it("accepts any candidate when nothing exists yet", () => {
    expect(isNewer({ updatedAt: 1, seq: 1 }, undefined)).toBe(true);
    expect(isNewer({ updatedAt: 1, seq: 1 }, null)).toBe(true);
  });
});

describe("BUILTIN_FIELD_BY_KEY", () => {
  it("maps the three built-in column keys to their legacy item fields", () => {
    expect(BUILTIN_FIELD_BY_KEY).toEqual({
      Priority: "priority",
      "Due Date": "dueDate",
      Status: "status",
    });
  });
});
```

- [ ] **Step 2: Run the tests to verify they fail**

Run: `npx vitest run tests/store/shape.test.js`
Expected: FAIL — `Failed to resolve import "../../src/store/shape.js"`.

- [ ] **Step 3: Write the implementation**

`src/store/shape.js`:

```js
// Normalised board state. Flat maps keyed by id so a single cell can be updated without
// touching anything else — the property the nested legacy blob lacks, and the reason
// granular realtime updates were impossible before this.

/**
 * Legacy items store the three built-in columns as top-level fields and every other
 * column under `custom`. The mapping is by column key, exactly as the legacy `gV()`
 * getter does it — so a user-created column named "Priority" resolves to the built-in
 * field, which is the pre-existing behaviour and is preserved deliberately.
 */
export const BUILTIN_FIELD_BY_KEY = {
  Priority: "priority",
  "Due Date": "dueDate",
  Status: "status",
};

export function createEmptyState() {
  return {
    boards: {},
    categories: {},
    items: {},
    columns: {},
    cells: {},
    view: {},
    seq: {},
  };
}

/**
 * Should `candidate` overwrite `existing`?
 *
 * Ordering is (serverTs, seq) lexicographic, which is exactly what the server's guarded
 * upsert does: `WHERE cells.updated_at <= EXCLUDED.updated_at` lets an equal timestamp
 * through, and `seq` is monotonic with arrival order, so comparing seq on a tie
 * reproduces the database's decision on the client. Client clocks are never consulted.
 *
 * Returns false for an exact (updatedAt, seq) match, which makes applying an op
 * idempotent and therefore safe to redeliver.
 */
export function isNewer(candidate, existing) {
  if (!existing) return true;
  if (candidate.updatedAt !== existing.updatedAt) {
    return candidate.updatedAt > existing.updatedAt;
  }
  return candidate.seq > existing.seq;
}
```

- [ ] **Step 4: Run the tests to verify they pass**

Run: `npx vitest run tests/store/shape.test.js`
Expected: PASS, 8 tests.

- [ ] **Step 5: Commit**

```bash
git add src/store/shape.js tests/store/shape.test.js
git commit -m "feat(store): normalised state shape and LWW ordering comparator

isNewer implements (serverTs, seq) lexicographic ordering, mirroring the
server's guarded upsert so client and server agree on every conflict.
Equal (updatedAt, seq) returns false, making op application idempotent.

Co-Authored-By: Lilly Code <lillycode@lilly.com>"
```

---

### Task 4: A legacy board fixture covering every field

Normalisation and its inverse are only trustworthy if the fixture they are tested against exercises every field the real data has. Building it as its own task keeps Tasks 5 and 6 focused, and gives the round-trip test something with real teeth.

**Files:**
- Create: `tests/fixtures/legacy-board.js`
- Test: `tests/fixtures/legacy-board.test.js`

**Interfaces:**
- Consumes: nothing.
- Produces: `legacyBoard()` returning a fresh deep copy of a complete legacy board object, and `legacyBoardKeys()` returning the sorted list of its top-level keys.

- [ ] **Step 1: Write the failing test**

`tests/fixtures/legacy-board.test.js`:

```js
import { describe, it, expect } from "vitest";
import { legacyBoard, legacyBoardKeys } from "./legacy-board.js";

describe("legacyBoard fixture", () => {
  it("carries every top-level field the real board data has", () => {
    expect(legacyBoardKeys()).toEqual([
      "archive",
      "calBorderCol",
      "calDotCol",
      "cC",
      "cClr",
      "cFl",
      "cO",
      "cT",
      "cW",
      "categories",
      "cc",
      "cols",
      "color",
      "crFl",
      "gFilters",
      "id",
      "name",
      "srtRules",
      "visCols",
    ]);
  });

  it("returns an independent copy each call", () => {
    const a = legacyBoard();
    a.categories[0].name = "mutated";
    expect(legacyBoard().categories[0].name).not.toBe("mutated");
  });

  it("has a header carrying its own field values, not just a name", () => {
    const header = legacyBoard().categories[0].headers[0];
    expect(header.priority).toBe("High");
    expect(header.dueDate).toBe("2026-09-01");
    expect(header.status).toBe("Active");
    expect(header.custom).toEqual({ Owner: "steven.chen", Effort: "3" });
  });

  it("has a custom column that exists in the registry but is not displayed", () => {
    const b = legacyBoard();
    expect(b.cc).toContain("Notes");
    expect(b.cols).not.toContain("Notes");
  });

  it("covers every column type", () => {
    const types = new Set(Object.values(legacyBoard().cT));
    expect(types).toEqual(new Set(["list", "date", "daterange", "text", "number", "checkbox"]));
  });
});
```

- [ ] **Step 2: Run to verify it fails**

Run: `npx vitest run tests/fixtures/legacy-board.test.js`
Expected: FAIL — module not found.

- [ ] **Step 3: Write the fixture**

`tests/fixtures/legacy-board.js`:

```js
// A legacy board exercising every field the real data carries, including the cases that
// break naive normalisation: a header with its own values, a registry column that is not
// displayed, a daterange value, and a numeric-looking string in `custom`.
//
// Field meanings, established by reading App.jsx:
//   cc      column registry (every column that exists)
//   cols    shared ordered display list; a subset of cc
//   cT      column key -> type
//   cO      column key -> list options
//   cW      column key -> pixel width; includes "Name", which is not a column
//   cClr    column key -> colour
//   cC      per-cell colour overrides
//   visCols per-user column visibility  (stripped on push)
//   gFilters / cFl / crFl / srtRules    per-user filters and sort (stripped on push)
//   calDotCol / calBorderCol            per-user calendar config (stripped on push)
//   archive shared list of removed items

const BOARD = {
  id: 1756100000000000,
  name: "Q3 Delivery",
  color: "bg-blue-500",

  cc: ["Priority", "Due Date", "Status", "Owner", "Effort", "Window", "Done", "Notes"],
  cols: ["Priority", "Due Date", "Status", "Owner", "Effort", "Window", "Done"],
  cT: {
    Priority: "list",
    "Due Date": "date",
    Status: "list",
    Owner: "text",
    Effort: "number",
    Window: "daterange",
    Done: "checkbox",
    Notes: "text",
  },
  cO: {
    Priority: ["None", "Low", "Medium", "High"],
    Status: ["None", "Active", "Pending", "Complete"],
  },
  cW: { Name: 250, Priority: 100, "Due Date": 130, Status: 100, Owner: 120 },
  cClr: { Priority: "bg-red-500" },
  cC: {},

  categories: [
    {
      id: 1756100000000001,
      name: "Platform",
      isExpanded: true,
      headers: [
        {
          id: 1756100000000002,
          name: "Migrate storage layer",
          priority: "High",
          dueDate: "2026-09-01",
          status: "Active",
          isExpanded: true,
          custom: { Owner: "steven.chen", Effort: "3" },
          subtasks: [
            {
              id: 1756100000000003,
              name: "Normalise board blob",
              priority: "Medium",
              dueDate: "2026-08-28",
              status: "Active",
              custom: { Owner: "steven.chen", Window: "2026-08-26|2026-08-28", Done: "true" },
            },
            {
              id: 1756100000000004,
              name: "Write round-trip test",
              priority: "None",
              dueDate: "",
              status: "None",
              custom: {},
            },
          ],
        },
        {
          id: 1756100000000005,
          name: "Empty header",
          priority: "None",
          dueDate: "",
          status: "None",
          isExpanded: false,
          custom: {},
          subtasks: [],
        },
      ],
    },
    {
      id: 1756100000000006,
      name: "Empty category",
      isExpanded: false,
      headers: [],
    },
  ],

  visCols: ["Priority", "Due Date", "Status"],
  gFilters: [
    {
      id: 1756100000000007,
      col: "Priority",
      op: "is",
      val: "High",
      val2: "",
      logic: "and",
      scope: "all",
    },
  ],
  cFl: { 1756100000000001: [] },
  crFl: {},
  srtRules: [{ col: "Due Date", dir: "asc" }],
  calDotCol: "Priority",
  calBorderCol: "Status",

  archive: [
    {
      id: 1756100000000008,
      name: "Dropped task",
      priority: "Low",
      dueDate: "",
      status: "None",
      custom: {},
      subtasks: [],
      _archivedAt: 1756000000000,
      _fromCategory: "Platform",
      _fromCategoryId: 1756100000000001,
    },
  ],
};

export function legacyBoard() {
  return structuredClone(BOARD);
}

export function legacyBoardKeys() {
  return Object.keys(BOARD).sort();
}
```

- [ ] **Step 4: Run to verify it passes**

Run: `npx vitest run tests/fixtures/legacy-board.test.js`
Expected: PASS, 5 tests.

- [ ] **Step 5: Commit**

```bash
git add tests/fixtures/
git commit -m "test: legacy board fixture covering every persisted field

Includes the cases that break naive normalisation: a header carrying its
own field values, a registry column absent from the display list, a
daterange value, and empty categories and headers.

Co-Authored-By: Lilly Code <lillycode@lilly.com>"
```

---

### Task 5: normalizeBoard — legacy blob to flat entities

**Files:**
- Create: `src/store/normalize.js`
- Test: `tests/store/normalize.test.js`

**Interfaces:**
- Consumes: `createEmptyState`, `BUILTIN_FIELD_BY_KEY` from `src/store/shape.js`; `legacyBoard` fixture.
- Produces: `normalizeBoard(legacy, {actorId = null, updatedAt = 0, seq = 0} = {}) -> State` — a complete `State` containing exactly one board.

- [ ] **Step 1: Write the failing tests**

`tests/store/normalize.test.js`:

```js
import { describe, it, expect } from "vitest";
import { normalizeBoard } from "../../src/store/normalize.js";
import { legacyBoard } from "../fixtures/legacy-board.js";

const B = 1756100000000000;
const CAT_PLATFORM = 1756100000000001;
const HDR_MIGRATE = 1756100000000002;
const ROW_NORMALISE = 1756100000000003;

describe("normalizeBoard", () => {
  it("creates one board record holding id order for categories and columns", () => {
    const s = normalizeBoard(legacyBoard());
    expect(Object.keys(s.boards)).toEqual([String(B)]);
    const b = s.boards[B];
    expect(b.name).toBe("Q3 Delivery");
    expect(b.color).toBe("bg-blue-500");
    expect(b.categoryIds).toEqual([CAT_PLATFORM, 1756100000000006]);
    expect(b.columnIds).toHaveLength(8);
  });

  it("flattens categories with array index as position", () => {
    const s = normalizeBoard(legacyBoard());
    expect(s.categories[CAT_PLATFORM]).toMatchObject({
      id: CAT_PLATFORM,
      boardId: B,
      name: "Platform",
      position: 0,
      isDeleted: false,
    });
    expect(s.categories[1756100000000006].position).toBe(1);
  });

  it("stores headers and rows in one items map distinguished by kind", () => {
    const s = normalizeBoard(legacyBoard());
    expect(s.items[HDR_MIGRATE]).toMatchObject({
      kind: "header",
      categoryId: CAT_PLATFORM,
      parentItemId: null,
      name: "Migrate storage layer",
      position: 0,
    });
    expect(s.items[ROW_NORMALISE]).toMatchObject({
      kind: "row",
      categoryId: CAT_PLATFORM,
      parentItemId: HDR_MIGRATE,
      name: "Normalise board blob",
      position: 0,
    });
  });

  it("gives headers cells, not just rows", () => {
    const s = normalizeBoard(legacyBoard());
    expect(s.cells[HDR_MIGRATE].Priority.value).toBe("High");
    expect(s.cells[HDR_MIGRATE]["Due Date"].value).toBe("2026-09-01");
    expect(s.cells[HDR_MIGRATE].Owner.value).toBe("steven.chen");
    expect(s.cells[HDR_MIGRATE].Effort.value).toBe("3");
  });

  it("reads built-in columns from top-level fields and the rest from custom", () => {
    const s = normalizeBoard(legacyBoard());
    const cells = s.cells[ROW_NORMALISE];
    expect(cells.Status.value).toBe("Active");
    expect(cells.Window.value).toBe("2026-08-26|2026-08-28");
  });

  it("never writes a Name cell — the name lives on the item", () => {
    const s = normalizeBoard(legacyBoard());
    expect(s.cells[ROW_NORMALISE].Name).toBeUndefined();
    expect(s.items[ROW_NORMALISE].name).toBe("Normalise board blob");
  });

  it("omits cells with no value rather than storing empty strings", () => {
    const s = normalizeBoard(legacyBoard());
    expect(s.cells[1756100000000004].Owner).toBeUndefined();
    expect(s.cells[1756100000000004]["Due Date"]).toBeUndefined();
  });

  it("keys columns by their legacy key and carries type, options, width, colour", () => {
    const s = normalizeBoard(legacyBoard());
    expect(s.columns.Priority).toMatchObject({
      id: "Priority",
      key: "Priority",
      label: "Priority",
      type: "list",
      options: ["None", "Low", "Medium", "High"],
      width: 100,
      color: "bg-red-500",
      position: 0,
      displayIndex: 0,
    });
    expect(s.columns.Owner.type).toBe("text");
    expect(s.columns.Owner.options).toEqual([]);
  });

  it("gives a registry column absent from cols a null displayIndex", () => {
    const s = normalizeBoard(legacyBoard());
    expect(s.columns.Notes.position).toBe(7);
    expect(s.columns.Notes.displayIndex).toBeNull();
  });

  it("moves per-user state into view and expansion out of the entities", () => {
    const s = normalizeBoard(legacyBoard());
    const v = s.view[B];
    expect(v.visCols).toEqual(["Priority", "Due Date", "Status"]);
    expect(v.calDotCol).toBe("Priority");
    expect(v.srtRules).toEqual([{ col: "Due Date", dir: "asc" }]);
    expect(v.expanded[CAT_PLATFORM]).toBe(true);
    expect(v.expanded[1756100000000006]).toBe(false);
    expect(v.expanded[HDR_MIGRATE]).toBe(true);
    expect(s.categories[CAT_PLATFORM].isExpanded).toBeUndefined();
    expect(s.items[HDR_MIGRATE].isExpanded).toBeUndefined();
  });

  it("carries archive verbatim on the board record", () => {
    const s = normalizeBoard(legacyBoard());
    expect(s.boards[B].archive).toHaveLength(1);
    expect(s.boards[B].archive[0]._fromCategory).toBe("Platform");
  });

  it("applies the migCC backfill for built-ins present in cols but missing from cc", () => {
    const legacy = legacyBoard();
    legacy.cc = ["Owner"];
    legacy.cols = ["Priority", "Owner", "Status"];
    const s = normalizeBoard(legacy);
    expect(s.boards[B].columnIds).toEqual(["Owner", "Priority", "Status"]);
  });

  it("stamps provenance on every cell so LWW has something to compare", () => {
    const s = normalizeBoard(legacyBoard(), { actorId: "u1", updatedAt: 500, seq: 7 });
    expect(s.cells[HDR_MIGRATE].Priority).toMatchObject({
      updatedAt: 500,
      updatedBy: "u1",
      seq: 7,
    });
    expect(s.seq[B]).toBe(7);
  });

  it("tolerates a board missing every optional field", () => {
    const s = normalizeBoard({ id: 9, name: "Bare" });
    expect(s.boards[9].categoryIds).toEqual([]);
    expect(s.boards[9].columnIds).toEqual([]);
    expect(s.view[9].gFilters).toEqual([]);
    expect(s.cells).toEqual({});
  });
});
```

- [ ] **Step 2: Run to verify they fail**

Run: `npx vitest run tests/store/normalize.test.js`
Expected: FAIL — module not found.

- [ ] **Step 3: Write the implementation**

`src/store/normalize.js`:

```js
import { createEmptyState, BUILTIN_FIELD_BY_KEY } from "./shape.js";

const BUILTIN_KEYS = ["Priority", "Due Date", "Status"];

/**
 * Reproduces the legacy `migCC` migration: any built-in column present in the display
 * list but missing from the registry is appended to the registry. Without this, boards
 * written by older versions of the Artifact lose columns on normalisation.
 */
function columnRegistry(cc, cols) {
  const out = [...(cc || [])];
  for (const key of BUILTIN_KEYS) {
    if ((cols || []).includes(key) && !out.includes(key)) out.push(key);
  }
  return out;
}

/** Read one column's value off a legacy item, by the same rules as the legacy `gV()`. */
function legacyValue(item, key) {
  const builtin = BUILTIN_FIELD_BY_KEY[key];
  if (builtin) return item[builtin];
  return item.custom?.[key];
}

function normalizeCells(state, item, columnKeys, stamp) {
  const cells = {};
  for (const key of columnKeys) {
    const value = legacyValue(item, key);
    // An absent or empty value is stored as no cell at all. Writing `""` everywhere would
    // triple the cell count for no gain and would make "never set" indistinguishable from
    // "explicitly cleared" — which matters once LWW starts comparing timestamps.
    if (value === undefined || value === null || value === "") continue;
    cells[key] = { value: String(value), ...stamp };
  }
  if (Object.keys(cells).length > 0) state.cells[item.id] = cells;
}

/**
 * Convert one legacy board object into normalised state.
 *
 * Per-user state (filters, sort, visible columns, calendar config, expansion) is lifted
 * out of the entities into `view`, matching what the legacy `stripViewState()` already
 * excluded from shared pushes. `archive` is carried verbatim on the board record; it is
 * modelled properly at M2.
 */
export function normalizeBoard(legacy, { actorId = null, updatedAt = 0, seq = 0 } = {}) {
  const state = createEmptyState();
  const boardId = legacy.id;
  const stamp = { updatedAt, updatedBy: actorId, seq };
  const entityStamp = { updatedAt, updatedBy: actorId, isDeleted: false };

  const registry = columnRegistry(legacy.cc, legacy.cols);
  const cols = legacy.cols || [];
  const cT = legacy.cT || {};
  const cO = legacy.cO || {};
  const cW = legacy.cW || {};
  const cClr = legacy.cClr || {};

  for (const [position, key] of registry.entries()) {
    const displayIndex = cols.indexOf(key);
    state.columns[key] = {
      id: key,
      boardId,
      key,
      label: key,
      type: cT[key] || "text",
      options: cO[key] || [],
      color: cClr[key] ?? null,
      width: cW[key] ?? null,
      position,
      displayIndex: displayIndex === -1 ? null : displayIndex,
      ...entityStamp,
    };
  }

  const expanded = {};
  const categoryIds = [];

  for (const [catPosition, cat] of (legacy.categories || []).entries()) {
    categoryIds.push(cat.id);
    expanded[cat.id] = cat.isExpanded !== false;
    state.categories[cat.id] = {
      id: cat.id,
      boardId,
      name: cat.name,
      position: catPosition,
      ...entityStamp,
    };

    for (const [hdrPosition, hdr] of (cat.headers || []).entries()) {
      expanded[hdr.id] = hdr.isExpanded === true;
      state.items[hdr.id] = {
        id: hdr.id,
        boardId,
        categoryId: cat.id,
        parentItemId: null,
        kind: "header",
        name: hdr.name,
        position: hdrPosition,
        ...entityStamp,
      };
      normalizeCells(state, hdr, registry, stamp);

      for (const [rowPosition, row] of (hdr.subtasks || []).entries()) {
        state.items[row.id] = {
          id: row.id,
          boardId,
          categoryId: cat.id,
          parentItemId: hdr.id,
          kind: "row",
          name: row.name,
          position: rowPosition,
          ...entityStamp,
        };
        normalizeCells(state, row, registry, stamp);
      }
    }
  }

  state.boards[boardId] = {
    id: boardId,
    name: legacy.name,
    color: legacy.color ?? null,
    folderId: legacy.folderId ?? null,
    archive: legacy.archive || [],
    nameWidth: cW.Name ?? null,
    cellColors: legacy.cC || {},
    categoryIds,
    columnIds: registry,
    ...entityStamp,
  };

  state.view[boardId] = {
    gFilters: legacy.gFilters || [],
    cFl: legacy.cFl || {},
    crFl: legacy.crFl || {},
    srtRules: legacy.srtRules || [],
    visCols: legacy.visCols || [],
    calDotCol: legacy.calDotCol ?? "Priority",
    calBorderCol: legacy.calBorderCol ?? "Status",
    expanded,
  };

  state.seq[boardId] = seq;
  return state;
}
```

- [ ] **Step 4: Run to verify they pass**

Run: `npx vitest run tests/store/normalize.test.js`
Expected: PASS, 14 tests.

- [ ] **Step 5: Commit**

```bash
git add src/store/normalize.js tests/store/normalize.test.js
git commit -m "feat(store): normalizeBoard, legacy blob to flat entities

Headers and rows collapse into one items map keyed by kind, because the
source gives them an identical field set and a shared value getter -
keying cells on rows alone would strand every header value. Per-user
state and expansion lift into view, mirroring stripViewState.

Co-Authored-By: Lilly Code <lillycode@lilly.com>"
```

---

### Task 6: denormalizeBoard and the round-trip property

The inverse of Task 5. This is the safety net for the whole plan: as long as `denormalize(normalize(x)) === x`, the store can become the source of truth without risking the data, and the app can keep persisting to `window.storage` through M1.

**Files:**
- Create: `src/store/denormalize.js`
- Test: `tests/store/denormalize.test.js`

**Interfaces:**
- Consumes: `BUILTIN_FIELD_BY_KEY`; `normalizeBoard`; `legacyBoard` fixture.
- Produces: `denormalizeBoard(state, boardId) -> legacyBoardObject`.

- [ ] **Step 1: Write the failing tests**

`tests/store/denormalize.test.js`:

```js
import { describe, it, expect } from "vitest";
import { normalizeBoard } from "../../src/store/normalize.js";
import { denormalizeBoard } from "../../src/store/denormalize.js";
import { legacyBoard } from "../fixtures/legacy-board.js";

const B = 1756100000000000;

describe("denormalizeBoard", () => {
  it("round-trips the full fixture unchanged", () => {
    const original = legacyBoard();
    const restored = denormalizeBoard(normalizeBoard(original), B);
    expect(restored).toEqual(original);
  });

  it("round-trips a board with no optional fields", () => {
    const bare = {
      id: 9,
      name: "Bare",
      color: null,
      categories: [],
      cc: [],
      cols: [],
      cT: {},
      cO: {},
      cW: {},
      cClr: {},
      cC: {},
      visCols: [],
      gFilters: [],
      cFl: {},
      crFl: {},
      srtRules: [],
      calDotCol: "Priority",
      calBorderCol: "Status",
      archive: [],
    };
    expect(denormalizeBoard(normalizeBoard(bare), 9)).toEqual(bare);
  });

  it("restores expansion from view state, not from the entities", () => {
    const s = normalizeBoard(legacyBoard());
    s.view[B].expanded[1756100000000001] = false;
    const restored = denormalizeBoard(s, B);
    expect(restored.categories[0].isExpanded).toBe(false);
  });

  it("writes built-ins to top-level fields and the rest to custom", () => {
    const restored = denormalizeBoard(normalizeBoard(legacyBoard()), B);
    const header = restored.categories[0].headers[0];
    expect(header.priority).toBe("High");
    expect(header.custom).toEqual({ Owner: "steven.chen", Effort: "3" });
  });

  it("omits soft-deleted entities", () => {
    const s = normalizeBoard(legacyBoard());
    s.items[1756100000000003].isDeleted = true;
    s.categories[1756100000000006].isDeleted = true;
    const restored = denormalizeBoard(s, B);
    expect(restored.categories).toHaveLength(1);
    expect(restored.categories[0].headers[0].subtasks).toHaveLength(1);
    expect(restored.categories[0].headers[0].subtasks[0].name).toBe("Write round-trip test");
  });

  it("orders by position, not by insertion order into the map", () => {
    const s = normalizeBoard(legacyBoard());
    s.categories[1756100000000001].position = 5;
    const restored = denormalizeBoard(s, B);
    expect(restored.categories.map((c) => c.name)).toEqual(["Empty category", "Platform"]);
  });

  it("reflects a cell written after normalisation", () => {
    const s = normalizeBoard(legacyBoard());
    s.cells[1756100000000004] = {
      ...(s.cells[1756100000000004] || {}),
      Owner: { value: "new.owner", updatedAt: 10, updatedBy: "u2", seq: 3 },
    };
    const restored = denormalizeBoard(s, B);
    const row = restored.categories[0].headers[0].subtasks[1];
    expect(row.custom.Owner).toBe("new.owner");
  });

  it("returns null for an unknown board", () => {
    expect(denormalizeBoard(normalizeBoard(legacyBoard()), 999)).toBeNull();
  });
});
```

- [ ] **Step 2: Run to verify they fail**

Run: `npx vitest run tests/store/denormalize.test.js`
Expected: FAIL — module not found.

- [ ] **Step 3: Write the implementation**

`src/store/denormalize.js`:

```js
import { BUILTIN_FIELD_BY_KEY } from "./shape.js";

const byPosition = (a, b) => a.position - b.position;

const live = (records) => records.filter((r) => !r.isDeleted).sort(byPosition);

/**
 * Rebuild one legacy item (header or row) from its item record and cells.
 *
 * Built-in columns are written back to top-level fields and default to the same values
 * the legacy `gV()` getter substituted for a missing value, so a normalise/denormalise
 * cycle produces byte-identical output rather than sprinkling nulls.
 */
function denormalizeItem(state, item, columnKeys) {
  const cells = state.cells[item.id] || {};
  const out = {
    id: item.id,
    name: item.name,
    priority: cells.Priority?.value ?? "None",
    dueDate: cells["Due Date"]?.value ?? "",
    status: cells.Status?.value ?? "None",
  };

  const custom = {};
  for (const key of columnKeys) {
    if (BUILTIN_FIELD_BY_KEY[key]) continue;
    const cell = cells[key];
    if (cell !== undefined) custom[key] = cell.value;
  }
  out.custom = custom;
  return out;
}

/**
 * Convert normalised state back into the legacy board object that App.jsx and
 * window.storage both expect. The inverse of normalizeBoard; the round-trip is asserted
 * against a fixture covering every field.
 */
export function denormalizeBoard(state, boardId) {
  const board = state.boards[boardId];
  if (!board) return null;

  const columnKeys = board.columnIds.filter((key) => !state.columns[key]?.isDeleted);
  const columns = columnKeys.map((key) => state.columns[key]);

  const cc = [...columnKeys].sort((a, b) => state.columns[a].position - state.columns[b].position);
  const cols = columns
    .filter((c) => c.displayIndex !== null)
    .sort((a, b) => a.displayIndex - b.displayIndex)
    .map((c) => c.key);

  const cT = {};
  const cO = {};
  const cW = {};
  const cClr = {};
  for (const c of columns) {
    cT[c.key] = c.type;
    if (c.options && c.options.length > 0) cO[c.key] = c.options;
    if (c.width !== null) cW[c.key] = c.width;
    if (c.color !== null) cClr[c.key] = c.color;
  }
  if (board.nameWidth !== null) cW.Name = board.nameWidth;

  const view = state.view[boardId] || {};
  const expanded = view.expanded || {};

  const allItems = Object.values(state.items).filter((i) => i.boardId === boardId);

  const categories = live(
    board.categoryIds.map((id) => state.categories[id]).filter(Boolean)
  ).map((cat) => ({
    id: cat.id,
    name: cat.name,
    isExpanded: expanded[cat.id] !== false,
    headers: live(
      allItems.filter((i) => i.kind === "header" && i.categoryId === cat.id)
    ).map((hdr) => ({
      ...denormalizeItem(state, hdr, columnKeys),
      isExpanded: expanded[hdr.id] === true,
      subtasks: live(allItems.filter((i) => i.parentItemId === hdr.id)).map((row) =>
        denormalizeItem(state, row, columnKeys)
      ),
    })),
  }));

  return {
    id: board.id,
    name: board.name,
    color: board.color,
    categories,
    cc,
    cols,
    cT,
    cO,
    cW,
    cClr,
    cC: board.cellColors,
    visCols: view.visCols || [],
    gFilters: view.gFilters || [],
    cFl: view.cFl || {},
    crFl: view.crFl || {},
    srtRules: view.srtRules || [],
    calDotCol: view.calDotCol,
    calBorderCol: view.calBorderCol,
    archive: board.archive,
  };
}
```

- [ ] **Step 4: Run to verify they pass**

Run: `npx vitest run tests/store/denormalize.test.js`
Expected: PASS, 8 tests. The first test — the full round-trip — is the one that matters; if it fails, the diff names the exact field that was lost.

- [ ] **Step 5: Run the whole suite**

Run: `npm test`
Expected: PASS, 35 tests across 4 files.

- [ ] **Step 6: Commit**

```bash
git add src/store/denormalize.js tests/store/denormalize.test.js
git commit -m "feat(store): denormalizeBoard and the legacy round-trip property

denormalize(normalize(board)) equals board for a fixture covering every
persisted field. This is what lets the store become the source of truth
without risking data, and what keeps window.storage working through M1.

Co-Authored-By: Lilly Code <lillycode@lilly.com>"
```

---

### Task 7: Selectors that reproduce the shapes the UI already consumes

This is the mechanism that keeps the 985-line component intact. `selectCategories(state, boardId)` returns the same nested array the component reads from its `cats` state today, so consumers can be repointed one at a time without rewriting render code.

**Files:**
- Create: `src/store/selectors.js`
- Test: `tests/store/selectors.test.js`

**Interfaces:**
- Consumes: `denormalizeBoard`.
- Produces:
  - `selectCategories(state, boardId) -> legacyCategoriesArray`
  - `selectColumnRegistry(state, boardId) -> string[]`  (legacy `ccl`)
  - `selectDisplayColumns(state, boardId) -> string[]`  (legacy `cols`)
  - `selectColumnMeta(state, boardId) -> {cT, cO, cW, cClr}`
  - `selectViewState(state, boardId) -> {gFilters, cFl, crFl, srtRules, visCols, calDotCol, calBorderCol}`
  - `selectCellValue(state, itemId, columnKey) -> string`
  - `selectCellProvenance(state, itemId, columnKey) -> {updatedAt, updatedBy, seq} | null`

- [ ] **Step 1: Write the failing tests**

`tests/store/selectors.test.js`:

```js
import { describe, it, expect } from "vitest";
import { normalizeBoard } from "../../src/store/normalize.js";
import {
  selectCategories,
  selectColumnRegistry,
  selectDisplayColumns,
  selectColumnMeta,
  selectViewState,
  selectCellValue,
  selectCellProvenance,
} from "../../src/store/selectors.js";
import { legacyBoard } from "../fixtures/legacy-board.js";

const B = 1756100000000000;
const HDR_MIGRATE = 1756100000000002;
const ROW_NORMALISE = 1756100000000003;

describe("selectCategories", () => {
  it("returns exactly the nested shape the legacy component renders", () => {
    const s = normalizeBoard(legacyBoard());
    expect(selectCategories(s, B)).toEqual(legacyBoard().categories);
  });

  it("returns an empty array for an unknown board rather than throwing", () => {
    expect(selectCategories(normalizeBoard(legacyBoard()), 999)).toEqual([]);
  });

  it("is referentially stable across calls when state has not changed", () => {
    const s = normalizeBoard(legacyBoard());
    expect(selectCategories(s, B)).toBe(selectCategories(s, B));
  });

  it("returns a new reference after a cell is written", () => {
    const s = normalizeBoard(legacyBoard());
    const before = selectCategories(s, B);
    const next = {
      ...s,
      cells: {
        ...s.cells,
        [ROW_NORMALISE]: {
          ...s.cells[ROW_NORMALISE],
          Owner: { value: "changed", updatedAt: 99, updatedBy: "u2", seq: 4 },
        },
      },
    };
    expect(selectCategories(next, B)).not.toBe(before);
  });
});

describe("column selectors", () => {
  it("splits the registry from the display list", () => {
    const s = normalizeBoard(legacyBoard());
    expect(selectColumnRegistry(s, B)).toEqual(legacyBoard().cc);
    expect(selectDisplayColumns(s, B)).toEqual(legacyBoard().cols);
  });

  it("returns the four legacy column metadata maps", () => {
    const s = normalizeBoard(legacyBoard());
    const meta = selectColumnMeta(s, B);
    expect(meta.cT).toEqual(legacyBoard().cT);
    expect(meta.cO).toEqual(legacyBoard().cO);
    expect(meta.cW).toEqual(legacyBoard().cW);
    expect(meta.cClr).toEqual(legacyBoard().cClr);
  });
});

describe("selectViewState", () => {
  it("returns per-user state without expansion, which the UI tracks separately", () => {
    const s = normalizeBoard(legacyBoard());
    const v = selectViewState(s, B);
    expect(v.visCols).toEqual(["Priority", "Due Date", "Status"]);
    expect(v.srtRules).toEqual([{ col: "Due Date", dir: "asc" }]);
    expect(v.expanded).toBeUndefined();
  });
});

describe("cell selectors", () => {
  it("returns the stored value", () => {
    const s = normalizeBoard(legacyBoard());
    expect(selectCellValue(s, HDR_MIGRATE, "Priority")).toBe("High");
  });

  it("substitutes the same defaults the legacy getter did", () => {
    const s = normalizeBoard(legacyBoard());
    expect(selectCellValue(s, 1756100000000004, "Priority")).toBe("None");
    expect(selectCellValue(s, 1756100000000004, "Status")).toBe("None");
    expect(selectCellValue(s, 1756100000000004, "Due Date")).toBe("");
    expect(selectCellValue(s, 1756100000000004, "Owner")).toBe("");
  });

  it("exposes provenance so a lost conflict can name the other editor", () => {
    const s = normalizeBoard(legacyBoard(), { actorId: "u1", updatedAt: 500, seq: 7 });
    expect(selectCellProvenance(s, HDR_MIGRATE, "Priority")).toEqual({
      updatedAt: 500,
      updatedBy: "u1",
      seq: 7,
    });
    expect(selectCellProvenance(s, HDR_MIGRATE, "Notes")).toBeNull();
  });
});
```

- [ ] **Step 2: Run to verify they fail**

Run: `npx vitest run tests/store/selectors.test.js`
Expected: FAIL — module not found.

- [ ] **Step 3: Write the implementation**

`src/store/selectors.js`:

```js
import { BUILTIN_FIELD_BY_KEY } from "./shape.js";
import { denormalizeBoard } from "./denormalize.js";

/**
 * Selectors deliberately return the *legacy* shapes rather than the normalised ones.
 * That is what allows the 985-line component to keep its render code: it goes on seeing
 * the nested arrays and keyed maps it already reads, and only the source changes.
 *
 * selectCategories rebuilds a nested tree, so it is memoised on identity of the four
 * slices it reads. Without this, every render would rebuild the whole tree and every
 * child would see new props.
 */
const categoryCache = new WeakMap();

export function selectCategories(state, boardId) {
  let perBoard = categoryCache.get(state);
  if (perBoard && boardId in perBoard) return perBoard[boardId];

  const board = state.boards[boardId];
  const result = board ? denormalizeBoard(state, boardId).categories : [];

  if (!perBoard) {
    perBoard = {};
    categoryCache.set(state, perBoard);
  }
  perBoard[boardId] = result;
  return result;
}

const columnsOf = (state, boardId) =>
  (state.boards[boardId]?.columnIds || [])
    .map((key) => state.columns[key])
    .filter((c) => c && !c.isDeleted);

export function selectColumnRegistry(state, boardId) {
  return columnsOf(state, boardId)
    .slice()
    .sort((a, b) => a.position - b.position)
    .map((c) => c.key);
}

export function selectDisplayColumns(state, boardId) {
  return columnsOf(state, boardId)
    .filter((c) => c.displayIndex !== null)
    .sort((a, b) => a.displayIndex - b.displayIndex)
    .map((c) => c.key);
}

export function selectColumnMeta(state, boardId) {
  const cT = {};
  const cO = {};
  const cW = {};
  const cClr = {};
  for (const c of columnsOf(state, boardId)) {
    cT[c.key] = c.type;
    if (c.options && c.options.length > 0) cO[c.key] = c.options;
    if (c.width !== null) cW[c.key] = c.width;
    if (c.color !== null) cClr[c.key] = c.color;
  }
  const nameWidth = state.boards[boardId]?.nameWidth;
  if (nameWidth !== null && nameWidth !== undefined) cW.Name = nameWidth;
  return { cT, cO, cW, cClr };
}

export function selectViewState(state, boardId) {
  const { expanded, ...rest } = state.view[boardId] || {};
  return rest;
}

/** Defaults match the legacy `gV()` getter exactly, so no rendered cell changes. */
export function selectCellValue(state, itemId, columnKey) {
  const cell = state.cells[itemId]?.[columnKey];
  if (cell !== undefined) return cell.value;
  if (BUILTIN_FIELD_BY_KEY[columnKey]) return columnKey === "Due Date" ? "" : "None";
  return "";
}

/** Who last wrote this cell and when — the input to the lost-conflict indicator. */
export function selectCellProvenance(state, itemId, columnKey) {
  const cell = state.cells[itemId]?.[columnKey];
  if (cell === undefined) return null;
  return { updatedAt: cell.updatedAt, updatedBy: cell.updatedBy, seq: cell.seq };
}
```

- [ ] **Step 4: Run to verify they pass**

Run: `npx vitest run tests/store/selectors.test.js`
Expected: PASS, 10 tests.

- [ ] **Step 5: Commit**

```bash
git add src/store/selectors.js tests/store/selectors.test.js
git commit -m "feat(store): selectors reproducing legacy render shapes

selectCategories returns the same nested array the component reads from
cats today, memoised per state object, so consumers can be repointed one
at a time without touching render code.

Co-Authored-By: Lilly Code <lillycode@lilly.com>"
```

---

### Task 8: Op constructors and `cell.set` with LWW

The first op kind, and the one that carries the conflict semantics. `applyOp` is a pure reducer: state in, state out, no mutation.

**Files:**
- Create: `src/store/ops.js`
- Test: `tests/store/ops.test.js`

**Interfaces:**
- Consumes: `isNewer` from `src/store/shape.js`.
- Produces:
  - `OP_KINDS` — the closed set from §5 of the spec
  - `cellSet({boardId, itemId, columnId, value, actorId, clientTs}) -> Op` (an op with no `serverTs`/`seq` yet)
  - `applyOp(state, op) -> State`

An `Op` is `{kind, boardId, target, payload, actorId, clientTs, serverTs, seq}`. For `cell.set`, `target` is `"<itemId>:<columnId>"` and `payload` is `{value}`.

- [ ] **Step 1: Write the failing tests**

`tests/store/ops.test.js`:

```js
import { describe, it, expect } from "vitest";
import { OP_KINDS, cellSet, applyOp } from "../../src/store/ops.js";
import { normalizeBoard } from "../../src/store/normalize.js";
import { selectCellValue, selectCellProvenance } from "../../src/store/selectors.js";
import { legacyBoard } from "../fixtures/legacy-board.js";

const B = 1756100000000000;
const HDR = 1756100000000002;

const serverOp = (op, serverTs, seq) => ({ ...op, serverTs, seq });

describe("OP_KINDS", () => {
  it("is the closed set the spec enumerates, plus row.rename", () => {
    expect([...OP_KINDS].sort()).toEqual([
      "board.rename",
      "category.delete",
      "category.insert",
      "category.move",
      "category.rename",
      "cell.set",
      "column.delete",
      "column.insert",
      "column.update",
      "header.delete",
      "header.insert",
      "header.move",
      "header.rename",
      "row.delete",
      "row.insert",
      "row.move",
      "row.rename",
    ]);
  });
});

describe("cellSet", () => {
  it("builds a cell.set op with a composite target and no server fields yet", () => {
    const op = cellSet({
      boardId: B,
      itemId: HDR,
      columnId: "Priority",
      value: "Low",
      actorId: "u1",
      clientTs: 111,
    });
    expect(op).toEqual({
      kind: "cell.set",
      boardId: B,
      target: `${HDR}:Priority`,
      payload: { value: "Low" },
      actorId: "u1",
      clientTs: 111,
      serverTs: null,
      seq: null,
    });
  });
});

describe("applyOp cell.set", () => {
  const base = () => normalizeBoard(legacyBoard(), { actorId: "u1", updatedAt: 100, seq: 1 });

  const setOp = (value, actorId = "u2") =>
    cellSet({ boardId: B, itemId: HDR, columnId: "Priority", value, actorId, clientTs: 0 });

  it("applies a newer write", () => {
    const next = applyOp(base(), serverOp(setOp("Low"), 200, 2));
    expect(selectCellValue(next, HDR, "Priority")).toBe("Low");
    expect(selectCellProvenance(next, HDR, "Priority")).toEqual({
      updatedAt: 200,
      updatedBy: "u2",
      seq: 2,
    });
  });

  it("rejects a stale write without mutating state", () => {
    const s = base();
    const next = applyOp(s, serverOp(setOp("Low"), 50, 0));
    expect(selectCellValue(next, HDR, "Priority")).toBe("High");
    expect(next).toBe(s);
  });

  it("breaks an equal timestamp by seq", () => {
    const s = applyOp(base(), serverOp(setOp("Low", "u2"), 200, 2));
    const winner = applyOp(s, serverOp(setOp("Medium", "u3"), 200, 3));
    expect(selectCellValue(winner, HDR, "Priority")).toBe("Medium");
    const loser = applyOp(s, serverOp(setOp("None", "u4"), 200, 1));
    expect(selectCellValue(loser, HDR, "Priority")).toBe("Low");
  });

  it("is idempotent — redelivering the same op is a no-op", () => {
    const s = applyOp(base(), serverOp(setOp("Low"), 200, 2));
    expect(applyOp(s, serverOp(setOp("Low"), 200, 2))).toBe(s);
  });

  it("never orders by clientTs", () => {
    const s = base();
    const fastClock = {
      ...serverOp(setOp("Low"), 50, 0),
      clientTs: 9_999_999_999_999,
    };
    expect(selectCellValue(applyOp(s, fastClock), HDR, "Priority")).toBe("High");
  });

  it("creates the cell map for an item that had no cells", () => {
    const bare = 1756100000000004;
    const op = cellSet({
      boardId: B,
      itemId: bare,
      columnId: "Owner",
      value: "new.owner",
      actorId: "u2",
      clientTs: 0,
    });
    const next = applyOp(base(), serverOp(op, 300, 5));
    expect(selectCellValue(next, bare, "Owner")).toBe("new.owner");
  });

  it("advances the board seq high-water mark", () => {
    const next = applyOp(base(), serverOp(setOp("Low"), 200, 42));
    expect(next.seq[B]).toBe(42);
  });

  it("does not lower the seq high-water mark on a rejected op", () => {
    const s = applyOp(base(), serverOp(setOp("Low"), 200, 42));
    const next = applyOp(s, serverOp(setOp("None"), 100, 7));
    expect(next.seq[B]).toBe(42);
  });

  it("leaves other cells and items untouched", () => {
    const s = base();
    const next = applyOp(s, serverOp(setOp("Low"), 200, 2));
    expect(next.items).toBe(s.items);
    expect(next.categories).toBe(s.categories);
    expect(next.cells[1756100000000003]).toBe(s.cells[1756100000000003]);
  });

  it("throws on an unknown op kind rather than silently dropping it", () => {
    expect(() => applyOp(base(), { kind: "nope", boardId: B, serverTs: 1, seq: 1 })).toThrow(
      /unknown op kind/i
    );
  });

  it("throws when a server-assigned field is missing", () => {
    expect(() => applyOp(base(), setOp("Low"))).toThrow(/serverTs/);
  });
});
```

- [ ] **Step 2: Run to verify they fail**

Run: `npx vitest run tests/store/ops.test.js`
Expected: FAIL — module not found.

- [ ] **Step 3: Write the implementation**

`src/store/ops.js`:

```js
import { isNewer } from "./shape.js";

/**
 * The closed set from §5 of the spec, enumerated so the replay path is exhaustive.
 *
 * `row.rename` is not in the spec's list but has to exist: a row's name is editable in
 * the UI exactly as a header's is, and without this kind there is no op that expresses
 * the edit. Recorded as a spec correction.
 */
export const OP_KINDS = new Set([
  "cell.set",
  "row.insert",
  "row.delete",
  "row.move",
  "row.rename",
  "header.insert",
  "header.rename",
  "header.delete",
  "header.move",
  "category.insert",
  "category.rename",
  "category.delete",
  "category.move",
  "column.insert",
  "column.update",
  "column.delete",
  "board.rename",
]);

export function cellSet({ boardId, itemId, columnId, value, actorId, clientTs }) {
  return {
    kind: "cell.set",
    boardId,
    target: `${itemId}:${columnId}`,
    payload: { value },
    actorId,
    clientTs,
    // Assigned by the server on receipt. The client never invents these: ordering is
    // server-authoritative, so a client-assigned timestamp would be a lie that a fast
    // laptop clock could use to win every conflict.
    serverTs: null,
    seq: null,
  };
}

function applyCellSet(state, op) {
  const [itemId, columnId] = op.target.split(":");
  const candidate = {
    value: op.payload.value,
    updatedAt: op.serverTs,
    updatedBy: op.actorId,
    seq: op.seq,
  };

  if (!isNewer(candidate, state.cells[itemId]?.[columnId])) return state;

  return {
    ...state,
    cells: {
      ...state.cells,
      [itemId]: { ...(state.cells[itemId] || {}), [columnId]: candidate },
    },
  };
}

const HANDLERS = {
  "cell.set": applyCellSet,
};

/**
 * Apply one server-ordered op. Pure: returns the same state object when the op is
 * rejected, so `next === prev` is a reliable "nothing changed" signal for React.
 */
export function applyOp(state, op) {
  if (!OP_KINDS.has(op.kind)) {
    throw new Error(`unknown op kind: ${op.kind}`);
  }
  if (op.serverTs === null || op.serverTs === undefined) {
    throw new Error("op is missing serverTs; ordering is server-authoritative");
  }
  if (op.seq === null || op.seq === undefined) {
    throw new Error("op is missing seq; ordering is server-authoritative");
  }

  const handler = HANDLERS[op.kind];
  if (!handler) {
    throw new Error(`op kind not yet implemented: ${op.kind}`);
  }

  const next = handler(state, op);
  if (next === state) return state;

  // The seq high-water mark only ever advances; it is what `Last-Event-ID` sends on
  // reconnect, so moving it backwards would replay ops already applied.
  const known = next.seq[op.boardId] ?? 0;
  if (op.seq > known) {
    return { ...next, seq: { ...next.seq, [op.boardId]: op.seq } };
  }
  return next;
}
```

- [ ] **Step 4: Run to verify they pass**

Run: `npx vitest run tests/store/ops.test.js`
Expected: PASS, 13 tests.

- [ ] **Step 5: Commit**

```bash
git add src/store/ops.js tests/store/ops.test.js
git commit -m "feat(store): op model and cell.set with server-authoritative LWW

applyOp refuses an op with no serverTs or seq, so a client cannot invent
ordering. Rejected ops return the identical state object, giving React a
reliable no-change signal. Redelivery is a no-op.

OP_KINDS adds row.rename, which the spec's enumeration omits despite a
row's name being editable exactly as a header's is.

Co-Authored-By: Lilly Code <lillycode@lilly.com>"
```

---

### Task 9: Structural ops and soft-delete tombstones

Insert, delete, move, and rename for categories, headers, rows, and columns. Deletion is `isDeleted: true` with an `updatedAt`, which replaces the legacy `_tombstones` array and its one-hour expiry window entirely — the window was the bug, not the mechanism.

**Files:**
- Modify: `src/store/ops.js`
- Test: `tests/store/ops-structural.test.js`

**Interfaces:**
- Consumes: `applyOp`, `isNewer`.
- Produces, all returning an `Op` with `serverTs: null, seq: null`:
  - `categoryInsert({boardId, id, name, position, actorId, clientTs})`
  - `categoryRename({boardId, id, name, actorId, clientTs})`
  - `categoryDelete({boardId, id, actorId, clientTs})`
  - `categoryMove({boardId, id, position, actorId, clientTs})`
  - `itemInsert({boardId, id, categoryId, parentItemId, kind, name, position, actorId, clientTs})` — emits `header.insert` when `kind === 'header'`, else `row.insert`
  - `itemRename({boardId, id, kind, name, actorId, clientTs})`
  - `itemDelete({boardId, id, kind, actorId, clientTs})`
  - `itemMove({boardId, id, kind, categoryId, parentItemId, position, actorId, clientTs})`
  - `columnInsert({boardId, key, type, options, position, displayIndex, actorId, clientTs})`
  - `columnUpdate({boardId, key, patch, actorId, clientTs})` — `patch` may set `label`, `type`, `options`, `width`, `color`, `displayIndex`
  - `columnDelete({boardId, key, actorId, clientTs})`
  - `boardRename({boardId, name, actorId, clientTs})`

- [ ] **Step 1: Write the failing tests**

`tests/store/ops-structural.test.js`:

```js
import { describe, it, expect } from "vitest";
import {
  applyOp,
  categoryInsert,
  categoryRename,
  categoryDelete,
  categoryMove,
  itemInsert,
  itemRename,
  itemDelete,
  itemMove,
  columnInsert,
  columnUpdate,
  columnDelete,
  boardRename,
} from "../../src/store/ops.js";
import { normalizeBoard } from "../../src/store/normalize.js";
import { selectCategories, selectColumnRegistry } from "../../src/store/selectors.js";
import { legacyBoard } from "../fixtures/legacy-board.js";

const B = 1756100000000000;
const CAT = 1756100000000001;
const HDR = 1756100000000002;
const ROW = 1756100000000003;

const base = () => normalizeBoard(legacyBoard(), { actorId: "u1", updatedAt: 100, seq: 1 });
const at = (op, serverTs, seq) => ({ ...op, serverTs, seq });
const A = { actorId: "u2", clientTs: 0 };

describe("category ops", () => {
  it("inserts a category and appends it to the board order", () => {
    const next = applyOp(
      base(),
      at(categoryInsert({ boardId: B, id: 777, name: "New", position: 2, ...A }), 200, 2)
    );
    expect(next.categories[777]).toMatchObject({ name: "New", position: 2, isDeleted: false });
    expect(next.boards[B].categoryIds).toContain(777);
    expect(selectCategories(next, B).map((c) => c.name)).toEqual([
      "Platform",
      "Empty category",
      "New",
    ]);
  });

  it("renames a category", () => {
    const next = applyOp(base(), at(categoryRename({ boardId: B, id: CAT, name: "Core", ...A }), 200, 2));
    expect(next.categories[CAT].name).toBe("Core");
  });

  it("soft-deletes rather than removing, and the selector hides it", () => {
    const next = applyOp(base(), at(categoryDelete({ boardId: B, id: CAT, ...A }), 200, 2));
    expect(next.categories[CAT].isDeleted).toBe(true);
    expect(next.categories[CAT].updatedAt).toBe(200);
    expect(selectCategories(next, B).map((c) => c.name)).toEqual(["Empty category"]);
  });

  it("moves a category by position", () => {
    const next = applyOp(base(), at(categoryMove({ boardId: B, id: CAT, position: 9, ...A }), 200, 2));
    expect(selectCategories(next, B).map((c) => c.name)).toEqual(["Empty category", "Platform"]);
  });

  it("rejects a stale rename", () => {
    const s = applyOp(base(), at(categoryRename({ boardId: B, id: CAT, name: "Core", ...A }), 200, 2));
    const next = applyOp(s, at(categoryRename({ boardId: B, id: CAT, name: "Old", ...A }), 150, 3));
    expect(next.categories[CAT].name).toBe("Core");
  });

  it("resurrects nothing — a delete then a stale rename stays deleted", () => {
    const s = applyOp(base(), at(categoryDelete({ boardId: B, id: CAT, ...A }), 300, 5));
    const next = applyOp(s, at(categoryRename({ boardId: B, id: CAT, name: "Zombie", ...A }), 200, 2));
    expect(next.categories[CAT].isDeleted).toBe(true);
    expect(next.categories[CAT].name).toBe("Platform");
  });
});

describe("item ops", () => {
  it("inserts a header", () => {
    const op = itemInsert({
      boardId: B,
      id: 888,
      categoryId: CAT,
      parentItemId: null,
      kind: "header",
      name: "Fresh",
      position: 2,
      ...A,
    });
    expect(op.kind).toBe("header.insert");
    const next = applyOp(base(), at(op, 200, 2));
    expect(next.items[888]).toMatchObject({ kind: "header", categoryId: CAT, name: "Fresh" });
  });

  it("inserts a row under a header", () => {
    const op = itemInsert({
      boardId: B,
      id: 889,
      categoryId: CAT,
      parentItemId: HDR,
      kind: "row",
      name: "Sub",
      position: 2,
      ...A,
    });
    expect(op.kind).toBe("row.insert");
    const next = applyOp(base(), at(op, 200, 2));
    expect(next.items[889].parentItemId).toBe(HDR);
    const subtasks = selectCategories(next, B)[0].headers[0].subtasks;
    expect(subtasks.map((r) => r.name)).toEqual([
      "Normalise board blob",
      "Write round-trip test",
      "Sub",
    ]);
  });

  it("soft-deletes a row and hides it from the selector", () => {
    const next = applyOp(base(), at(itemDelete({ boardId: B, id: ROW, kind: "row", ...A }), 200, 2));
    expect(next.items[ROW].isDeleted).toBe(true);
    expect(selectCategories(next, B)[0].headers[0].subtasks).toHaveLength(1);
  });

  it("leaves orphaned rows hidden when their header is deleted", () => {
    const next = applyOp(
      base(),
      at(itemDelete({ boardId: B, id: HDR, kind: "header", ...A }), 200, 2)
    );
    expect(selectCategories(next, B)[0].headers.map((h) => h.name)).toEqual(["Empty header"]);
    // The rows are not themselves deleted; they are simply unreachable, which is what
    // makes an undelete of the header restore its children.
    expect(next.items[ROW].isDeleted).toBe(false);
  });

  it("renames an item", () => {
    const next = applyOp(
      base(),
      at(itemRename({ boardId: B, id: ROW, kind: "row", name: "Renamed", ...A }), 200, 2)
    );
    expect(next.items[ROW].name).toBe("Renamed");
  });

  it("reparents a row to a different header", () => {
    const other = 1756100000000005;
    const next = applyOp(
      base(),
      at(
        itemMove({
          boardId: B,
          id: ROW,
          kind: "row",
          categoryId: CAT,
          parentItemId: other,
          position: 0,
          ...A,
        }),
        200,
        2
      )
    );
    expect(next.items[ROW].parentItemId).toBe(other);
    const cats = selectCategories(next, B);
    expect(cats[0].headers[0].subtasks).toHaveLength(1);
    expect(cats[0].headers[1].subtasks[0].name).toBe("Normalise board blob");
  });
});

describe("column ops", () => {
  it("inserts a column into the registry and the display list", () => {
    const next = applyOp(
      base(),
      at(
        columnInsert({
          boardId: B,
          key: "Risk",
          type: "list",
          options: ["Low", "High"],
          position: 8,
          displayIndex: 7,
          ...A,
        }),
        200,
        2
      )
    );
    expect(selectColumnRegistry(next, B)).toContain("Risk");
    expect(next.columns.Risk.type).toBe("list");
  });

  it("patches only the fields given", () => {
    const next = applyOp(
      base(),
      at(columnUpdate({ boardId: B, key: "Owner", patch: { width: 300 }, ...A }), 200, 2)
    );
    expect(next.columns.Owner.width).toBe(300);
    expect(next.columns.Owner.type).toBe("text");
  });

  it("soft-deletes a column and hides it from the registry", () => {
    const next = applyOp(base(), at(columnDelete({ boardId: B, key: "Owner", ...A }), 200, 2));
    expect(next.columns.Owner.isDeleted).toBe(true);
    expect(selectColumnRegistry(next, B)).not.toContain("Owner");
  });

  it("keeps the cells of a deleted column so an undelete restores its data", () => {
    const next = applyOp(base(), at(columnDelete({ boardId: B, key: "Owner", ...A }), 200, 2));
    expect(next.cells[HDR].Owner.value).toBe("steven.chen");
  });
});

describe("board.rename", () => {
  it("renames the board", () => {
    const next = applyOp(base(), at(boardRename({ boardId: B, name: "Q4 Delivery", ...A }), 200, 2));
    expect(next.boards[B].name).toBe("Q4 Delivery");
  });
});
```

- [ ] **Step 2: Run to verify they fail**

Run: `npx vitest run tests/store/ops-structural.test.js`
Expected: FAIL — the named exports do not exist.

- [ ] **Step 3: Add the constructors and handlers to `src/store/ops.js`**

Append to `src/store/ops.js`:

```js
// --- op constructors -------------------------------------------------------------

const op = (kind, boardId, target, payload, { actorId, clientTs }) => ({
  kind,
  boardId,
  target,
  payload,
  actorId,
  clientTs,
  serverTs: null,
  seq: null,
});

export const categoryInsert = ({ boardId, id, name, position, actorId, clientTs }) =>
  op("category.insert", boardId, id, { name, position }, { actorId, clientTs });

export const categoryRename = ({ boardId, id, name, actorId, clientTs }) =>
  op("category.rename", boardId, id, { name }, { actorId, clientTs });

export const categoryDelete = ({ boardId, id, actorId, clientTs }) =>
  op("category.delete", boardId, id, {}, { actorId, clientTs });

export const categoryMove = ({ boardId, id, position, actorId, clientTs }) =>
  op("category.move", boardId, id, { position }, { actorId, clientTs });

export const itemInsert = ({
  boardId,
  id,
  categoryId,
  parentItemId,
  kind,
  name,
  position,
  actorId,
  clientTs,
}) =>
  op(
    kind === "header" ? "header.insert" : "row.insert",
    boardId,
    id,
    { categoryId, parentItemId, kind, name, position },
    { actorId, clientTs }
  );

export const itemRename = ({ boardId, id, kind, name, actorId, clientTs }) =>
  op(
    kind === "header" ? "header.rename" : "row.rename",
    boardId,
    id,
    { name },
    { actorId, clientTs }
  );

export const itemDelete = ({ boardId, id, kind, actorId, clientTs }) =>
  op(kind === "header" ? "header.delete" : "row.delete", boardId, id, {}, { actorId, clientTs });

export const itemMove = ({
  boardId,
  id,
  kind,
  categoryId,
  parentItemId,
  position,
  actorId,
  clientTs,
}) =>
  op(
    kind === "header" ? "header.move" : "row.move",
    boardId,
    id,
    { categoryId, parentItemId, position },
    { actorId, clientTs }
  );

export const columnInsert = ({
  boardId,
  key,
  type,
  options,
  position,
  displayIndex,
  actorId,
  clientTs,
}) =>
  op(
    "column.insert",
    boardId,
    key,
    { key, type, options, position, displayIndex },
    { actorId, clientTs }
  );

export const columnUpdate = ({ boardId, key, patch, actorId, clientTs }) =>
  op("column.update", boardId, key, { patch }, { actorId, clientTs });

export const columnDelete = ({ boardId, key, actorId, clientTs }) =>
  op("column.delete", boardId, key, {}, { actorId, clientTs });

export const boardRename = ({ boardId, name, actorId, clientTs }) =>
  op("board.rename", boardId, boardId, { name }, { actorId, clientTs });

// --- handlers --------------------------------------------------------------------

const stampOf = (o) => ({ updatedAt: o.serverTs, updatedBy: o.actorId, seq: o.seq });

/**
 * Update one record in one entity map, guarded by LWW.
 *
 * `patch` may be a function of the existing record. Returns the original state when the
 * op is stale, so a late-arriving rename cannot resurrect a deleted entity or undo a
 * newer edit.
 */
function guardedUpdate(state, mapName, id, o, patch) {
  const map = state[mapName];
  const existing = map[id];
  const stamp = stampOf(o);

  if (existing && !isNewer(stamp, { updatedAt: existing.updatedAt, seq: existing.seq ?? 0 })) {
    return state;
  }

  const fields = typeof patch === "function" ? patch(existing) : patch;
  return {
    ...state,
    [mapName]: {
      ...map,
      [id]: { ...(existing || {}), ...fields, updatedAt: o.serverTs, updatedBy: o.actorId, seq: o.seq },
    },
  };
}

/** Append an id to a board's ordering array, idempotently. */
function withBoardListEntry(state, boardId, listName, id) {
  const board = state.boards[boardId];
  if (!board || board[listName].includes(id)) return state;
  return {
    ...state,
    boards: { ...state.boards, [boardId]: { ...board, [listName]: [...board[listName], id] } },
  };
}

Object.assign(HANDLERS, {
  "category.insert": (s, o) => {
    const next = guardedUpdate(s, "categories", o.target, o, {
      id: o.target,
      boardId: o.boardId,
      name: o.payload.name,
      position: o.payload.position,
      isDeleted: false,
    });
    return withBoardListEntry(next, o.boardId, "categoryIds", o.target);
  },
  "category.rename": (s, o) => guardedUpdate(s, "categories", o.target, o, { name: o.payload.name }),
  "category.delete": (s, o) => guardedUpdate(s, "categories", o.target, o, { isDeleted: true }),
  "category.move": (s, o) =>
    guardedUpdate(s, "categories", o.target, o, { position: o.payload.position }),

  "board.rename": (s, o) => guardedUpdate(s, "boards", o.target, o, { name: o.payload.name }),

  "column.insert": (s, o) => {
    const next = guardedUpdate(s, "columns", o.target, o, {
      id: o.payload.key,
      boardId: o.boardId,
      key: o.payload.key,
      label: o.payload.key,
      type: o.payload.type || "text",
      options: o.payload.options || [],
      color: null,
      width: null,
      position: o.payload.position,
      displayIndex: o.payload.displayIndex ?? null,
      isDeleted: false,
    });
    return withBoardListEntry(next, o.boardId, "columnIds", o.target);
  },
  "column.update": (s, o) => guardedUpdate(s, "columns", o.target, o, o.payload.patch),
  "column.delete": (s, o) =>
    // Cells are deliberately left in place. A column delete that discarded its data would
    // make undelete impossible and would turn a misclick into permanent loss.
    guardedUpdate(s, "columns", o.target, o, { isDeleted: true }),
});

const itemInsertHandler = (s, o) =>
  guardedUpdate(s, "items", o.target, o, {
    id: o.target,
    boardId: o.boardId,
    categoryId: o.payload.categoryId,
    parentItemId: o.payload.parentItemId ?? null,
    kind: o.payload.kind,
    name: o.payload.name,
    position: o.payload.position,
    isDeleted: false,
  });

const itemRenameHandler = (s, o) => guardedUpdate(s, "items", o.target, o, { name: o.payload.name });

const itemDeleteHandler = (s, o) => guardedUpdate(s, "items", o.target, o, { isDeleted: true });

const itemMoveHandler = (s, o) =>
  guardedUpdate(s, "items", o.target, o, {
    categoryId: o.payload.categoryId,
    parentItemId: o.payload.parentItemId ?? null,
    position: o.payload.position,
  });

Object.assign(HANDLERS, {
  "header.insert": itemInsertHandler,
  "row.insert": itemInsertHandler,
  "header.rename": itemRenameHandler,
  "row.rename": itemRenameHandler,
  "header.delete": itemDeleteHandler,
  "row.delete": itemDeleteHandler,
  "header.move": itemMoveHandler,
  "row.move": itemMoveHandler,
});
```

Two things to watch. `HANDLERS` and `isNewer` must both be in scope — `HANDLERS` is declared with `const` in Task 8 above these `Object.assign` calls, and `Object.assign` mutates that same object, so the order of the declarations in the file does not matter as long as everything is at module scope. And `guardedUpdate` reads `existing.seq ?? 0`, because `normalizeBoard` stamps entity records with `updatedAt` and `updatedBy` but no `seq` — without the fallback, `isNewer` compares against `undefined` and every guarded update silently fails.

- [ ] **Step 4: Run to verify they pass**

Run: `npx vitest run tests/store/ops-structural.test.js tests/store/ops.test.js`
Expected: PASS.

- [ ] **Step 5: Run the whole suite**

Run: `npm test`
Expected: PASS, all files.

- [ ] **Step 6: Commit**

```bash
git add src/store/ops.js tests/store/ops-structural.test.js tests/store/ops.test.js
git commit -m "feat(store): structural ops with soft-delete tombstones

isDeleted plus updatedAt replaces the legacy _tombstones array and its
one-hour expiry window, which was the mechanism by which deleted rows
came back. A stale rename can no longer resurrect a deleted entity.
Column deletes keep their cells so undelete restores data.

Co-Authored-By: Lilly Code <lillycode@lilly.com>"
```

---

### Task 10: `applyOps` — ordered, idempotent replay

What a reconnecting SSE client does with the batch the server replays after `Last-Event-ID`. Must be order-insensitive on input (the server sends ascending, but a retry can interleave) and idempotent.

**Files:**
- Modify: `src/store/ops.js`
- Test: `tests/store/replay.test.js`

**Interfaces:**
- Consumes: `applyOp`.
- Produces: `applyOps(state, ops) -> State`, sorting by `(serverTs, seq)` before applying.

- [ ] **Step 1: Write the failing tests**

`tests/store/replay.test.js`:

```js
import { describe, it, expect } from "vitest";
import { applyOps, cellSet, categoryRename } from "../../src/store/ops.js";
import { normalizeBoard } from "../../src/store/normalize.js";
import { selectCellValue } from "../../src/store/selectors.js";
import { legacyBoard } from "../fixtures/legacy-board.js";

const B = 1756100000000000;
const HDR = 1756100000000002;

const base = () => normalizeBoard(legacyBoard(), { actorId: "u1", updatedAt: 100, seq: 1 });
const set = (value, serverTs, seq) => ({
  ...cellSet({ boardId: B, itemId: HDR, columnId: "Priority", value, actorId: "u2", clientTs: 0 }),
  serverTs,
  seq,
});

describe("applyOps", () => {
  it("applies a batch in order and lands on the last value", () => {
    const next = applyOps(base(), [set("Low", 200, 2), set("Medium", 300, 3), set("None", 400, 4)]);
    expect(selectCellValue(next, HDR, "Priority")).toBe("None");
    expect(next.seq[B]).toBe(4);
  });

  it("is insensitive to input order", () => {
    const ops = [set("Low", 200, 2), set("Medium", 300, 3), set("None", 400, 4)];
    const forward = applyOps(base(), ops);
    const shuffled = applyOps(base(), [ops[2], ops[0], ops[1]]);
    expect(selectCellValue(shuffled, HDR, "Priority")).toBe("None");
    expect(shuffled.cells[HDR].Priority).toEqual(forward.cells[HDR].Priority);
  });

  it("is idempotent — replaying the same batch changes nothing", () => {
    const ops = [set("Low", 200, 2), set("Medium", 300, 3)];
    const once = applyOps(base(), ops);
    const twice = applyOps(once, ops);
    expect(twice).toBe(once);
  });

  it("tolerates an overlapping replay after a reconnect", () => {
    const first = applyOps(base(), [set("Low", 200, 2), set("Medium", 300, 3)]);
    const next = applyOps(first, [set("Medium", 300, 3), set("None", 400, 4)]);
    expect(selectCellValue(next, HDR, "Priority")).toBe("None");
    expect(next.seq[B]).toBe(4);
  });

  it("returns the same state object for an empty batch", () => {
    const s = base();
    expect(applyOps(s, [])).toBe(s);
  });

  it("interleaves ops of different kinds by server order", () => {
    const next = applyOps(base(), [
      { ...categoryRename({ boardId: B, id: 1756100000000001, name: "Core", actorId: "u2", clientTs: 0 }), serverTs: 250, seq: 3 },
      set("Low", 200, 2),
    ]);
    expect(next.categories[1756100000000001].name).toBe("Core");
    expect(selectCellValue(next, HDR, "Priority")).toBe("Low");
  });
});
```

- [ ] **Step 2: Run to verify they fail**

Run: `npx vitest run tests/store/replay.test.js`
Expected: FAIL — `applyOps` is not exported.

- [ ] **Step 3: Implement `applyOps`**

Append to `src/store/ops.js`:

```js
/**
 * Apply a batch of server-ordered ops.
 *
 * Sorts by (serverTs, seq) before applying, so a batch that arrives out of order — an
 * SSE reconnect overlapping a live push, say — converges on the same state as the
 * in-order case. Combined with per-op idempotence, replaying an overlapping window is
 * safe, which is what makes `Last-Event-ID` reconnection exact.
 */
export function applyOps(state, ops) {
  if (!ops || ops.length === 0) return state;
  const ordered = [...ops].sort((a, b) =>
    a.serverTs !== b.serverTs ? a.serverTs - b.serverTs : a.seq - b.seq
  );
  return ordered.reduce(applyOp, state);
}
```

- [ ] **Step 4: Run to verify they pass**

Run: `npx vitest run tests/store/replay.test.js`
Expected: PASS, 6 tests.

- [ ] **Step 5: Commit**

```bash
git add src/store/ops.js tests/store/replay.test.js
git commit -m "feat(store): applyOps, order-insensitive idempotent replay

Sorts by (serverTs, seq) then folds applyOp, so an SSE reconnect that
overlaps a live push converges on the same state as the in-order case.
This is what makes Last-Event-ID replay exact.

Co-Authored-By: Lilly Code <lillycode@lilly.com>"
```

---

### Task 11: Offline op queue

Ops made while disconnected are held, then flushed in order on reconnect. Kept separate from the store so the transport swap contemplated in §12 of the spec touches only `src/sync/`.

**Files:**
- Create: `src/sync/queue.js`
- Test: `tests/sync/queue.test.js`

**Interfaces:**
- Consumes: nothing.
- Produces: `createQueue() -> {enqueue(op), pending(), flush(send) -> Promise<{sent, failed}>, size()}` where `send(op) -> Promise<serverAssignedOp>`.

- [ ] **Step 1: Write the failing tests**

`tests/sync/queue.test.js`:

```js
import { describe, it, expect, vi } from "vitest";
import { createQueue } from "../../src/sync/queue.js";

const op = (n) => ({ kind: "cell.set", target: `1:Priority`, payload: { value: String(n) } });

describe("createQueue", () => {
  it("starts empty", () => {
    const q = createQueue();
    expect(q.size()).toBe(0);
    expect(q.pending()).toEqual([]);
  });

  it("preserves enqueue order", () => {
    const q = createQueue();
    q.enqueue(op(1));
    q.enqueue(op(2));
    expect(q.pending().map((o) => o.payload.value)).toEqual(["1", "2"]);
  });

  it("sends in order and empties on success", async () => {
    const q = createQueue();
    q.enqueue(op(1));
    q.enqueue(op(2));
    const seen = [];
    const send = vi.fn(async (o) => {
      seen.push(o.payload.value);
      return { ...o, serverTs: 1, seq: seen.length };
    });
    const result = await q.flush(send);
    expect(seen).toEqual(["1", "2"]);
    expect(result.sent).toHaveLength(2);
    expect(result.failed).toEqual([]);
    expect(q.size()).toBe(0);
  });

  it("stops at the first failure and keeps the rest queued in order", async () => {
    const q = createQueue();
    q.enqueue(op(1));
    q.enqueue(op(2));
    q.enqueue(op(3));
    const send = vi.fn(async (o) => {
      if (o.payload.value === "2") throw new Error("offline");
      return { ...o, serverTs: 1, seq: 1 };
    });
    const result = await q.flush(send);
    expect(result.sent).toHaveLength(1);
    expect(result.failed).toHaveLength(1);
    // Ops after the failure are never attempted: sending 3 before 2 would reorder the
    // user's edits, and ordering is the one thing the queue exists to preserve.
    expect(send).toHaveBeenCalledTimes(2);
    expect(q.pending().map((o) => o.payload.value)).toEqual(["2", "3"]);
  });

  it("returns the server-assigned ops so the caller can apply them", async () => {
    const q = createQueue();
    q.enqueue(op(1));
    const { sent } = await q.flush(async (o) => ({ ...o, serverTs: 500, seq: 9 }));
    expect(sent[0]).toMatchObject({ serverTs: 500, seq: 9 });
  });

  it("is a no-op when empty", async () => {
    const send = vi.fn();
    const result = await createQueue().flush(send);
    expect(send).not.toHaveBeenCalled();
    expect(result).toEqual({ sent: [], failed: [] });
  });

  it("refuses concurrent flushes so ops cannot be sent twice", async () => {
    const q = createQueue();
    q.enqueue(op(1));
    let release;
    const gate = new Promise((r) => (release = r));
    const send = vi.fn(async (o) => {
      await gate;
      return { ...o, serverTs: 1, seq: 1 };
    });
    const first = q.flush(send);
    const second = await q.flush(send);
    expect(second).toEqual({ sent: [], failed: [] });
    release();
    await first;
    expect(send).toHaveBeenCalledTimes(1);
  });
});
```

- [ ] **Step 2: Run to verify they fail**

Run: `npx vitest run tests/sync/queue.test.js`
Expected: FAIL — module not found.

- [ ] **Step 3: Write the implementation**

`src/sync/queue.js`:

```js
/**
 * An in-order, at-least-once queue for ops made while disconnected.
 *
 * Deliberately not persisted: at M1 there is no server to flush to, and persisting a
 * queue whose ops carry no serverTs would mean replaying them against a future backend
 * with no way to order them. M2 revisits this once the server assigns ordering.
 *
 * Ops are safe to retry because every handler is idempotent under (serverTs, seq).
 */
export function createQueue() {
  let queue = [];
  let flushing = false;

  return {
    enqueue(op) {
      queue.push(op);
    },

    pending() {
      return [...queue];
    },

    size() {
      return queue.length;
    },

    /**
     * Send queued ops one at a time, in order, stopping at the first failure.
     *
     * Stopping matters: skipping a failed op to send the next one would reorder the
     * user's edits, and under last-write-wins a reordering is a silent wrong answer
     * rather than a visible error.
     */
    async flush(send) {
      if (flushing || queue.length === 0) return { sent: [], failed: [] };
      flushing = true;
      const sent = [];
      const failed = [];
      try {
        while (queue.length > 0) {
          const op = queue[0];
          try {
            sent.push(await send(op));
            queue.shift();
          } catch (err) {
            failed.push({ op, error: err });
            break;
          }
        }
      } finally {
        flushing = false;
      }
      return { sent, failed };
    },
  };
}
```

- [ ] **Step 4: Run to verify they pass**

Run: `npx vitest run tests/sync/queue.test.js`
Expected: PASS, 7 tests.

- [ ] **Step 5: Commit**

```bash
git add src/sync/queue.js tests/sync/queue.test.js
git commit -m "feat(sync): in-order offline op queue

Stops at the first failure rather than skipping ahead: reordering edits
under last-write-wins produces a silently wrong value instead of a
visible error. Not persisted at M1 — ops carry no serverTs yet.

Co-Authored-By: Lilly Code <lillycode@lilly.com>"
```

---

### Task 12: Zustand store with `window.storage` persistence, and one consumer switched

Wires the pure functions into a live store, persists through the existing `window.storage` contract via `denormalizeBoard`, and switches **one** read path in `App.jsx` — the categories list — to `selectCategories`. Switching one consumer proves the selector contract against the real UI, which is the whole point of building selectors that return legacy shapes. The remaining consumers are the M2 plan's opening tasks.

**Files:**
- Create: `src/store/index.js`
- Test: `tests/store/index.test.js`
- Modify: `src/App.jsx` (categories read path only)

**Interfaces:**
- Consumes: `createEmptyState`, `normalizeBoard`, `denormalizeBoard`, `applyOp`, `applyOps`, selectors.
- Produces a Zustand store with:
  - `state` — the normalised `State`
  - `loadBoard(legacyBoard)` — normalise and merge one board
  - `dispatchLocal(op)` — apply optimistically with a provisional `(serverTs, seq)`, and return the op
  - `applyRemote(ops)` — apply a server batch
  - `toLegacy(boardId)` — `denormalizeBoard` for persistence
  - `useCategories(boardId)` — React hook wrapping `selectCategories`

- [ ] **Step 1: Write the failing tests**

`tests/store/index.test.js`:

```js
import { describe, it, expect, beforeEach } from "vitest";
import { useBoardStore } from "../../src/store/index.js";
import { cellSet } from "../../src/store/ops.js";
import { legacyBoard } from "../fixtures/legacy-board.js";

const B = 1756100000000000;
const HDR = 1756100000000002;

describe("useBoardStore", () => {
  beforeEach(() => useBoardStore.getState().reset());

  it("starts empty", () => {
    expect(useBoardStore.getState().state.boards).toEqual({});
  });

  it("loads a legacy board", () => {
    useBoardStore.getState().loadBoard(legacyBoard());
    expect(useBoardStore.getState().state.boards[B].name).toBe("Q3 Delivery");
  });

  it("round-trips a loaded board back to legacy shape", () => {
    const original = legacyBoard();
    useBoardStore.getState().loadBoard(original);
    expect(useBoardStore.getState().toLegacy(B)).toEqual(original);
  });

  it("applies a local op optimistically and returns it stamped", () => {
    const store = useBoardStore.getState();
    store.loadBoard(legacyBoard());
    const returned = store.dispatchLocal(
      cellSet({ boardId: B, itemId: HDR, columnId: "Priority", value: "Low", actorId: "me", clientTs: 1 })
    );
    expect(returned.serverTs).not.toBeNull();
    expect(returned.seq).not.toBeNull();
    expect(useBoardStore.getState().state.cells[HDR].Priority.value).toBe("Low");
  });

  it("lets a server echo with a real serverTs override the optimistic value", () => {
    const store = useBoardStore.getState();
    store.loadBoard(legacyBoard());
    const local = store.dispatchLocal(
      cellSet({ boardId: B, itemId: HDR, columnId: "Priority", value: "Low", actorId: "me", clientTs: 1 })
    );
    useBoardStore.getState().applyRemote([
      { ...local, payload: { value: "Medium" }, actorId: "someone-else", serverTs: local.serverTs + 1000, seq: local.seq + 1 },
    ]);
    expect(useBoardStore.getState().state.cells[HDR].Priority.value).toBe("Medium");
    expect(useBoardStore.getState().state.cells[HDR].Priority.updatedBy).toBe("someone-else");
  });

  it("gives each local op a strictly increasing provisional seq", () => {
    const store = useBoardStore.getState();
    store.loadBoard(legacyBoard());
    const mk = (v) =>
      cellSet({ boardId: B, itemId: HDR, columnId: "Priority", value: v, actorId: "me", clientTs: 0 });
    const a = store.dispatchLocal(mk("Low"));
    const b = useBoardStore.getState().dispatchLocal(mk("Medium"));
    expect(b.seq).toBeGreaterThan(a.seq);
    expect(useBoardStore.getState().state.cells[HDR].Priority.value).toBe("Medium");
  });
});
```

- [ ] **Step 2: Run to verify they fail**

Run: `npx vitest run tests/store/index.test.js`
Expected: FAIL — module not found.

- [ ] **Step 3: Write the store**

`src/store/index.js`:

```js
import { create } from "zustand";
import { createEmptyState } from "./shape.js";
import { normalizeBoard } from "./normalize.js";
import { denormalizeBoard } from "./denormalize.js";
import { applyOp, applyOps } from "./ops.js";
import { selectCategories } from "./selectors.js";

/**
 * Provisional ordering for an optimistic local op.
 *
 * At M1 there is no server, so a local op needs *some* (serverTs, seq) to be applied at
 * all. `Date.now()` plus a monotonic counter is used, and it is provisional by design:
 * any real server echo carries a later serverTs and therefore wins. This is the one
 * place a client clock is consulted, and only to order a client against itself.
 */
let provisionalSeq = 0;
const nextProvisional = () => ({ serverTs: Date.now(), seq: ++provisionalSeq });

export const useBoardStore = create((set, get) => ({
  state: createEmptyState(),

  reset() {
    provisionalSeq = 0;
    set({ state: createEmptyState() });
  },

  loadBoard(legacy, { actorId = null } = {}) {
    const incoming = normalizeBoard(legacy, { actorId, updatedAt: 0, seq: 0 });
    const current = get().state;
    set({
      state: {
        boards: { ...current.boards, ...incoming.boards },
        categories: { ...current.categories, ...incoming.categories },
        items: { ...current.items, ...incoming.items },
        columns: { ...current.columns, ...incoming.columns },
        cells: { ...current.cells, ...incoming.cells },
        view: { ...current.view, ...incoming.view },
        seq: { ...current.seq, ...incoming.seq },
      },
    });
  },

  dispatchLocal(op) {
    const stamped = { ...op, ...nextProvisional() };
    set({ state: applyOp(get().state, stamped) });
    return stamped;
  },

  applyRemote(ops) {
    set({ state: applyOps(get().state, ops) });
  },

  toLegacy(boardId) {
    return denormalizeBoard(get().state, boardId);
  },
}));

/** Hook form for the one consumer switched in this task. */
export const useCategories = (boardId) =>
  useBoardStore((s) => (boardId ? selectCategories(s.state, boardId) : []));
```

- [ ] **Step 4: Run to verify they pass**

Run: `npx vitest run tests/store/index.test.js`
Expected: PASS, 6 tests.

- [ ] **Step 5: Switch the categories read path in `App.jsx`**

Find the `cats` state declaration and its setter:

```bash
grep -n "sCats\|\[cats," src/App.jsx | head -20
```

Make the minimal change: keep `sCats` and every write path exactly as they are, and add a store load alongside them so the store is populated in parallel. Then read from the store for rendering.

Add near the top of `App.jsx`:

```jsx
import { useBoardStore, useCategories } from "./store/index.js";
```

Immediately after the existing `sCats(...)` calls in the board-load path, mirror the load into the store:

```jsx
// M1: the store is loaded in parallel with the legacy `cats` state. Both are live; the
// store is the read source for rendering, `cats` remains the write path until M2
// repoints the mutations. Keeping both means the app is runnable at every commit.
useBoardStore.getState().loadBoard(b);
```

Then replace the render-time read of `cats` with the selector:

```jsx
const storeCats = useCategories(activeBoard);
const renderCats = storeCats.length > 0 ? storeCats : cats;
```

Now repoint the render sites. Enumerate every read of `cats` first:

```bash
grep -no "sCats([^)]*\|cats\.\(map\|filter\|find\|forEach\|reduce\|some\|length\)" src/App.jsx
```

Classify each hit by where its result goes — this is mechanical, not a judgement call:

- **Rendering** — the expression's value flows into JSX (it is inside the `return (...)` of the component, or inside a `useMemo`/local `const` that only JSX consumes). Repoint these to `renderCats`.
- **Mutation** — the expression's value flows into `sCats(...)`, or it appears inside a handler that calls `sCats`. Leave these on `cats`. A `cats.map(...)` nested *inside* a `sCats(...)` call is a mutation read even though it looks identical.

Stop when `grep -c "renderCats" src/App.jsx` is non-zero and `grep -n "sCats(" src/App.jsx` returns the same line count it returned before this step — the second check is the guard that proves no write path moved.

- [ ] **Step 6: Verify no behaviour changed**

Run: `npm run build && npm run dev`
Expected: build succeeds. In the browser, repeat the full Task 2 Step 5 checklist: add a category, add a header, add a row, set Priority/Due Date/Status on both header and row, collapse and expand, filter, sort, calendar view, Gantt view, reload.
Expected: identical behaviour. If rows vanish, `renderCats` is being read where a mutation expected `cats` — revert that site.

- [ ] **Step 7: Run the full suite**

Run: `npm test`
Expected: PASS, all files.

- [ ] **Step 8: Commit**

```bash
git add src/store/index.js tests/store/index.test.js src/App.jsx
git commit -m "feat(store): Zustand store and the first consumer on selectors

The categories render path now reads selectCategories while every write
path still goes through the legacy cats state, so the app is runnable at
this commit. Proves the selector contract against the real UI before M2
repoints the remaining consumers.

Local ops get a provisional (serverTs, seq) from the client clock, used
only to order the client against itself; any server echo carries a later
serverTs and wins.

Co-Authored-By: Lilly Code <lillycode@lilly.com>"
```

---

## Definition of done

- `npm run build` succeeds; `npm run dev` serves a styled, working board.
- `npm test` passes — 94 tests across 10 files (8 shape, 5 fixture, 14 normalize, 8 denormalize, 10 selectors, 13 ops, 17 structural ops, 6 replay, 7 queue, 6 store).
- `denormalizeBoard(normalizeBoard(board)) === board` for a fixture covering every persisted field.
- LWW resolution, stale-op rejection, soft-delete tombstones, and ordered idempotent replay are all unit-tested with no browser and no cluster, per §11 of the spec.
- The categories render path reads through a selector; behaviour is unchanged.
- Two spec corrections are recorded for the author: `cells` must key on items (headers carry values), and `ops.kind` needs `row.rename`.

## Not in this plan

Per §10 of the spec, these are separate plans:

- **M2** — FastAPI, asyncpg, migrations, `/ops`, `/stream`, the auth adapter and `AUTHPROBE`, the Dockerfile and manifests, and repointing the remaining `App.jsx` consumers off `window.storage`. First shareable URL.
- **M3** — Redis pub/sub fan-out, `minReplicas: 2`, HPA on `kind: Rollout`.
- **M4** — S3 snapshots and restore-to-new-board.
- **M5** — Cortex adapter, `pypdf`, tool-call translation.

Also out of scope here, and noted so it is not forgotten: `archive` is carried as an opaque board field rather than normalised into `items`; the storage shim makes sharing single-browser; and the offline queue is not persisted.

One thing in this plan runs slightly ahead of the spec's M1 line: `src/sync/queue.js` (Task 11) is built and tested before there is any server to flush to, and nothing in M0–M1 imports it. It is here because its ordering rule is the part most likely to be got wrong under deadline pressure in M2, and it is pure enough to test now for free. If you would rather defer it, dropping Task 11 costs nothing else in this plan.
