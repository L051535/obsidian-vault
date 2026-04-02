---
date:
type: reference
status:
tags:
  - type/reference
  - topic/git
---

**git pull origin main --allow-unrelated-histories**

| Part                          | Meaning                                                                                                                                              |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| `git pull`                    | Download changes from remote and merge them into your local repo                                                                                     |
| `origin`                      | The name of your remote (GitHub) — "origin" is the default alias                                                                                     |
| `main`                        | The branch you're pulling from                                                                                                                       |
| `--allow-unrelated-histories` | GitHub repo was created independently with a README, so the two repos have no shared history. This flag forces Git to merge them anyway despite that |
|                               |                                                                                                                                                      |

---

**git push -u origin main**

| Part       | Meaning                                                                                                                                                 |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `git push` | Upload your local commits to the remote                                                                                                                 |
| `-u`       | Short for `--set-upstream` — links your local `main` to `origin/main` so future pushes/pulls just need `git push` or `git pull` with no extra arguments |
| `origin`   | The remote to push to (GitHub)                                                                                                                          |
| `main`     | The branch you're pushing                                                                                                                               |
|            |                                                                                                                                                         |

#topic/git