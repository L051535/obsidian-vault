---
doc_number: PRD-86821
version: "25.0"
status: Effective
effective_date: 2025-08-19
training_id: USD-SOP-Stock Withdrawal-001
tags:
  - nalo/sop
  - nalo/stock-withdrawal
---

# Stock Withdrawal of Finished Products

**Supersedes:** 001-001113, 001-001086, 006680
**Areas Involved:** North American Logistics Operations (NALO) Operations, North American Shared Service Center – Customer Service and Inventory Management (NASSC-CS), NALO Quality Assurance

## Purpose

To describe requirements for ad-hoc stock withdrawal of finished products after it has been sent to US or Canadian Affiliate distribution operations inventory. This procedure outlines the requirements for the request, the documentation, and the system transactions necessary for these stock withdrawals within the United States, Canada and Puerto Rico.

## Approvals

Approval of this procedure requires the signatures of:

- Management, NALO Operations
- Management, NASSC-CS
- Management, NALO Quality Assurance

## References

- CS-SOP-13892, Handling and Completion of DEA Form 222s by Purchasers and Suppliers (US)
- USD-SOP-FDA Product Sampling-001
- USD-SOP-Processing Orders Governed by Registry-077
- USD-SOP-NASSC_CS Sales Order for Trade Product Entry-012
- USD-SOP-Phys Trnfr Form-001
- Global Records Retention Schedule
- CS-SOP-002, Transfer of Controlled Substances (US)
- CS-FRM-002-01, Controlled Substance Transfer Form (CSTF)
- CS-JOB-002-03, Verification of DEA Registrations for Controlled Substance Transfers

## Reasons for Revision

All revisions are marked with an (*) in the source document:

1. To update hyperlinks and change "Controlled Drugs" to "Controlled Substance" throughout the procedure.
   - Rationale: To reflect current terminology.
2. To remove Zyprexa Relprev.
   - Rationale: Lilly no longer manufactures it.
3. To update that the Account assignment/cost center is listed on the Stock Withdrawal form in the "Request is for US Affiliate (e.g. Brand Team)" If/Then section of Step 9 in the Entering Product Requests into SAP section.
   - Rationale: For clarification.

## Introduction

On occasion, requests are made from many areas throughout the company or from third parties to withdraw finished stock from inventory under the control of a US, Canadian, or Puerto Rican Affiliate. Though each area is not listed, the scope of this procedure covers processing any finished stock withdrawal request from any distribution center owned by Lilly or contracted by a Lilly Affiliate to distribute finished stock to our first paying customers. Third parties include partner companies that own the stock in a U.S. or Canadian distribution – no costing is associated with the material or its withdrawal.

Requests to withdraw any finished product item should be submitted as follows and require the following approvals:

1. Requestor submits a written request to NASSC-CS via Attachment A, NALO Finished Stock Withdrawal Form or Attachment B, NALO Finished Stock Withdrawal Form-Flip Logic Clinical Trials. Either form must be signed/dated by the Requestor AND Requestor's supervision, or owner of budget being charged.
2. Written approval from Management in NALO / NALO QA is required unless otherwise noted.
3. Return instructions are provided on Attachment A, NALO Finished Stock Withdrawal Form and Attachment B, NALO Finished Stock Withdrawal Form-Flip Logic Clinical Trials.

> [!note]
> Attachment A, NALO Finished Stock Withdrawal Form, Attachment B, NALO Finished Stock Withdrawal Form-Flip Logic Clinical Trials and any associated documentation (order forms, etc.) are managed in accordance with the Global Records Retention Schedule.

**Exceptions to this process:**

- **FDA Request:** If the request is an FDA sample request where the agency comes to the warehouse to pull the samples, follow USD-SOP-FDA Product Sampling-001 instead of this procedure.
- **Clinical Trial Material:** Alternate form/process is utilized directly from the third party requestor.
- **Lilly Pharmacy:** Alternate form/process is utilized.

> [!note]
> CS product(s) may NOT be sent via interoffice mail or by regular mail.

> [!note]
> PDC Operations follows USD-SOP-Phys Trnfr Form-001, if needed, to transfer product locally.

> [!note]
> The following attachments reside within the electronic document management system:
> - Attachment A, NALO Finished Stock Withdrawal Form (USD-SOP-Stock Withdrawal-001 Att A)
> - Attachment B, NALO Finished Stock Withdrawal Form-Flip Logic Clinical Trials (USD-SOP-Stock Withdrawal-001 Att B)

## Additional Approvals

Additional approvals or verification may be necessary based on the product being requested:

**If the request is for a Controlled Substance (CS):**

- For Controlled Substance requests for Schedule II, a Drug Enforcement Administration (DEA) Form 222 is required to be attached to Attachment A or B. Contact Global Quality Auditing and Compliance (GQAAC) for the appropriate blank DEA Form 222 if needed.
- For all Controlled Substance transfers, a Controlled Substance Transfer Form (CSTF) (available on the GQAAC sharepoint site) must be completed in accordance to CS-SOP-002, Transfer of Controlled Substances (US) if all required information on this form is not printed on the packlist. The form must be attached to Attachment A or B.
  - If the requestor is not a Lilly employee, consult with the CS Officer regarding proper documentation requirements.
  - If the requestor is an employee or a contractor working at Lilly, check the Controlled Substance Authorization List to confirm the requestor is authorized to handle CS product(s).
  - If the requestor is a Third-Party company, check the Quality or other contractual agreement. If contractual language includes CS requirements, product(s) may be shipped to Third Party Operations (TPO). If contractual language does not exist, CS may not be shipped to the TPO until such verbiage is included in the revised agreement.

> [!note]
> Sales Representatives are not allowed to receive Controlled Substance.

If the request is for a **PROMOTIONAL SAMPLE** that is NOT being sent to a Lilly sales representative or licensed prescriber, written approval is needed from Consumer Product Quality Assurance / Sample Operations.

> [!note]
> If promotional sample is a CS item, ensure that CS steps above are performed.

> [!note]
> As a courtesy, it is recommended that the NASSC-CS inform a member of US Brand team responsible for the promotional sample item of the request to withdraw promotional samples (assuming a US Brand member was not the requestor).

## Entering Product Requests into SAP

Upon receipt of a request to withdraw product from finished goods inventory, NASSC-CS will enter an order into SAP:

| Step | Action |
|------|--------|
| 1 | Review Attachment A or B to ensure completeness based on product(s) being requested. If form is not complete, follow-up with requestor. Ensure that approval from NALO inventory management and NALO management signature has been obtained.<br><br>If the request includes Controlled Substance (CS), proceed to section "Additional Requirements if the Request Includes a Controlled Substance (US Only)". |
| 2 | Start creation of an order in SAP by selecting the following transaction code and/or menu path: `Logistics>Sales and Distribution>Sales>Order>VA01 – Create Order` |
| 3 | Determine Order Type (see If/Then table below). |

**Step 3 — Determine Order Type:**

| If | Then |
|----|------|
| For US/PR and the requestor **WILL** be charged for the product | If order is for Clinical Trial Product: if Trial Alias specified on the order form contains xxx-MC-xxx, when prompted to select an order type, use order type **Z14**. Else, use order type **Z12**. |
| For US/PR and the requestor will **NOT** be charged for the product | If order is NOT for Clinical Trial Product, use order type = **Z14**. |
| For Canada | If order is NOT for Clinical Trial Product: use order type = **Z10**.<br>Note: If the request is for product to be shipped to a partner company/third party who already owns the stock, a Z10 order type must be used to ensure that no charges are associated with the order.<br>If order is for Clinical Trial Product, use order type **Z12**.<br>If order is NOT for Clinical Trial Product: use **Z10** free of charge for materials that do not contain medical ingredient; use **Z11** for orders that are not charged but are intended to generate further revenue and are not for the full period of the treatment (i.e., samples) — Note: Cost center can be overwritten at time of order entry on the order; use **Z13** for orders that are not to be charged but are not intended to generate further revenue and can be for the full period of the treatment (i.e., patient is unable to afford the product).<br>Please consult with the Canadian Affiliate if there are any questions about the correct order type. |

| Step | Action |
|------|--------|
| 4 | Enter Sales Organization/Distribution Channel/Division for order (see If/Then table below). |

| If | Then use |
|----|----------|
| US Affiliate trade or sample product | Sales organization = 1100, Distribution Channel = 01, Division = 01 (Pharma) |
| Puerto Rico trade product | Sales organization = 1501, Distribution Channel = 01, Division = 01 (Pharma) |
| Puerto Rico sample product | Sales organization = 1100, Distribution Channel = 03, Division = 01 (Pharma) |
| Canadian product | Sales organization = 1429, Distribution Channel = 01, Division = 01 (Pharma) |

| Step | Action |
|------|--------|
| 5 | Click the Green Check or hit the Enter button to go to the next screen. The standard order entry screen appears. This screen is the same for all order types. |
| 6 | Enter Purchase Order number if applicable. For US Clinical Trial orders – the PO is typically the Trial Alias – e.g. B5K-MC-IBDS. Else, enter the Requestor's name unless form specifies another option. |
| 7 | Enter the Sold To party. To search for the customer; use the dropdown box or press F4 in the Sold To party field (see If/Then table below). |

| If | Then |
|----|------|
| For US | If order is for Clinical Trial Product, Sold To = 191769 - CLINICAL FIELD TRIALS - LILLY. Else, if requestor has a Sold To SAP Customer Number, use it; else, Sold To = One Time Customer Number 56000089. |
| For PR | If requestor has a Sold To SAP Customer Number, use it. Else, contact BPKC to determine correct SAP One Time Customer Number to be used. |
| For Canada | If order is for Clinical Trial Product, Sold To = 56000045 - CLINICAL FIELD TRIALS - LILLY. Else, if requestor has a Sold To SAP Customer Number, use it; else, Sold To = One Time Customer Number 56000043. |

| Step | Action |
|------|--------|
| 8 | After selecting Sold To customer number, hit Enter and the Ship To customer number with associated information will default into the Ship To customer field. For US Clinical Trial orders, Ship To is typically = 427250 - FISHER CLINICAL SERVICES.<br><br>Note: For One-time customers, typically the Sold To and the Ship To information is the same.<br><br>If the Ship To does not populate with the recipient's name/address designated on the order form, see Customer Master data steward for guidance. |
| 9 | Before entering line items, complete the following: `GOTO>HEADER>ACCOUNT ASSIGNMENT`. Enter the appropriate accounting code (XXXXXX). Save order. (See If/Then table below.) |

| If | Then |
|----|------|
| Request is from a Lilly Employee who does not work for the US Affiliate (manufacturing, engineering, labs, etc.) | If order is for Clinical Trial Product: if Trial Alias specified on the order form contains xxx-MC-xxx, use assignment/cost center 100ACTP; else, the account assignment is automatically determined in SAP. If order is NOT for Clinical Trial Product: use the cost center specified on the request form for the Account assignment/cost center. |
| Request is for US Affiliate (e.g. Brand Team) | Account assignment/cost center is listed on the Stock Withdrawal form. |
| Request is for US Sales Force | Account assignment/cost center is 100A042. |

| Step | Action |
|------|--------|
| 10 | Enter Requested Delivery Date, if needed. Note: If the order MUST ship to arrive on a certain date, NASSC coordinates with the appropriate Operations team to ensure this happens. If the requested delivery date cannot be met, NASSC contacts the requestor to discuss options. Note: Saturday deliveries require notification to appropriate Operations team and NALO Logistics plus the collection of a contact name and phone number of the recipient who will be at the delivery location on Saturday – this information will be entered on the order and printed on the shipping label to assist with delivery if needed. |
| 11 | Check Shipping Route and modify, if needed. A route may be modified on an order to expedite service (i.e. go from shipping ground to next day air), or to better utilize available transportation options (i.e. consolidate shipments, service outages, etc.). See list of commonly used routes on NASSC Customer Service Collaboration site. When there is an electronic feed of orders from Lilly to a third-party warehouse, such as the Canadian UPS warehouse, send an e-mail to outside warehouse if the route is changed.<br><br>Note: If product needs to be shipped to Internal Local Lilly employee, Shipping Condition: Emergency & Route: USLOCL.<br><br>Note: If product is controlled substance and is being shipped internally, the delivery cannot occur over the weekend or on a holiday. The CS Authorized personnel requesting the CS product shipment must be available to receive the delivery and complete the CSTF if provided. |
| 12 | Enter the appropriate Shipping Condition (if needed) by selecting the following transaction code and/or menu path: `Goto>Header>Shipping>Shipping Conditions`. Most orders are set at Standard shipping conditions. If the situation or the customer requires the shipment to be sent in a manner other than the standard shipping process, change the shipping condition to 'Emergency' for US and PR orders and to 'Express' for CA Orders when overriding the automatic route determination. This ensures that the delivery has the same route as the order. |
| 13 | Enter the Item Code in the Material field. Press Tab. If specific batches or serialized units are required, enter batch to be picked and/or Serialized product information to be selected. |
| 14 | Enter the quantity in the Order Quantity field and press Enter to go to the next line. |
| 15 | Continue to enter items per Steps 14–17 until the order is complete. |
| 16 | Complete the NALO Customer Service Use Only portion of the form from Attachment A or Attachment B. |
| 17 | Save supporting documentation. |
| 18 | Note: If order is for a controlled substance, you MUST attach a .pdf or screen shot of DEA website containing Ship To's validated DEA registration information to SAP sales order. |

**Directions for saving supporting documentation:**

> [!note]
> For controlled substance, send copy of documentation to CS officer for filing if a CSTF is completed. A CSTF is not required if all the information on this form is printed on the packlist.

| If | Then |
|----|------|
| File hard copy of document(s) | File in appropriate departmental file in accordance with Global Records Retention Schedule. |
| Attach electronic copy of document(s) to sales order | In SAP, open order in change mode (VA02). Go to Systems -> Services for Object. Select icon Create Attachment. Find document to attach to the file and click on Open. |

## Additional Requirements if the Request Includes a Controlled Substance (US Only)

If the product request includes a Controlled Substance (CS), NASSC-CS will follow the steps below:

| Step | Action |
|------|--------|
| 1 | If a Lilly employee requests CS product(s): verify/validate DEA registration number either through the DEA website or GQAAC's sharepoint site for the site requesting the shipment; check the Controlled Substance Authorization List to verify the requestor or intended recipient is authorized to handle CS items. |
| 2 | If Third Party/External Owner requests CS product(s): contact External Manufacturing Team or Alliance Management Team as they are responsible for determining if the agreement with Lilly includes CS requirements, and — if product requested is a Controlled Substance — that the following conditions are true: Third Party/Receiver has a current DEA registration; Ship To address matches DEA registration address; DEA Registration has not expired; and DEA registration includes the Schedule of CS being requested. External Manufacturing Team or Alliance Management Team provides NASSC-CS with Receiver's DEA Registration Number. Obtain signature of External Manufacturing Team or Alliance Management Team on Attachment A, NALO Finished Stock Withdrawal Form or attach documentation of approval with request paperwork and note approval on Attachment A. |
| 3 | If the product is a Schedule II Controlled Substance: obtain a DEA Form 222 from the requestor prior to shipping (reference CS-SOP-13893, Handling and Completion of DEA Form 222 for Suppliers (US)). Note: Product(s) can only be shipped to Receiver's DEA registered address. Validate that Receiver's DEA license is valid by accessing the DEA Website: Ship To address matches Receiver's DEA registered address; DEA Registration has not expired; and DEA registration includes the Schedule of CS being requested. Ensure that a .pdf or screen shot of DEA Website containing Ship To's validated DEA registration information is attached to SAP sales order. Obtain a Controlled Substance Transfer Form 002-01 (available on the GQAAC's sharepoint site). Provide both forms to NALO Operations or third-party distribution operations to complete and send to the requestor with the product. Send a photocopy or scanned copy of the DEA Form 222 to GQAAC for ARCOS reporting. |
| 4 | Return to Entering Product Requests into SAP Section. |

## Document Approval Signatures

| Name | Role | Verdict | Date |
|------|------|---------|------|
| Sabry Samuel (C039290@lilly.com) | Quality Approver | Approve | 29-Jul-2025 18:06:23 GMT+0000 |
| Eric Thomas (L107260@lilly.com) | Approver | Approve | 29-Jul-2025 18:13:50 GMT+0000 |
| Brandon Bowler (L004253@lilly.com) | Approver | Approve | 31-Jul-2025 21:47:15 GMT+0000 |

Reviewer / Approver Additional Details: N/A

---

*Source: PRD-86821, Version 25.0, Effective 19 Aug 2025. Approved 31 Jul 2025 by Brandon Bowler. Converted from PDF for reference — always verify current version in QualityDocs before relying on this for compliance decisions.*
