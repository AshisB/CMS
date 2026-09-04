# Canteen Management System (CMS)
### Requirement Analysis Document — Version 1.0 (Draft)

**Prepared for:** Office Canteen Digitization Project
**Document Status:** Draft — for internal team review
**Date:** September 2026

---

## 1. Introduction

This document captures the requirements for a Canteen Management System (CMS) intended to digitize the office canteen's ordering, billing, and monthly reimbursement process. It replaces informal or manual tracking with a structured application that manages employee food allowances, daily menus, order fulfillment, and month-end billing to the GS (General Services) Department.

The project is being developed as a learning initiative, led by one team member with a supporting junior developer. Requirements are intentionally scoped in phases to keep the first version achievable while leaving room for the system to grow.

## 2. Scope

### 2.1 In Scope — Version 1 (V1)

- Individual user accounts (employee, vendor, intern, guest, etc.) each with a configurable monthly spending limit
- Monthly allowance cycle aligned to the Nepali calendar
- Daily menu display and browsing
- Order placement, cancellation (pre-processing), and status tracking
- Admin-controlled menu creation and order assignment to waiters
- Department budget accounts (e.g. shared department tea budget), built as second priority within V1
- Month-end calculation and report submission to GS Department
- In-charge/department-head dashboard features, including two-factor (Google Authenticator/TOTP) verification of generated bills

### 2.2 Out of Scope — Deferred to Phase 2+

- Inventory management
- Dealer / vendor contact and purchase-list management
- Decentralized menu control (cooks creating menus, helpers setting wait times)
- Waiter self-editing of delivery time estimates
- Per-role separate logins for canteen sub-staff (order taker, cook, washer, waiter) — V1 uses a single shared Admin login

## 3. Actors / User Roles

| Actor | Description |
|---|---|
| **Consumer (Employee)** | Browses the daily menu, places and cancels orders, tracks order status, and has a monthly spending balance. |
| **Vendor / Intern / Guest / Board / CEO** | Special account types. Currently uncapped in practice, but the system should support a configurable limit per account type rather than one flat allowance for everyone. |
| **Canteen Staff** | Umbrella role covering Order Taker, Waiter, Cook, and Washer. In V1, these are handled by a single Admin login; the system design should not block splitting them into separate logins later. |
| **Canteen Admin** | Creates/edits the daily menu, reviews incoming orders, moves an order to "Processing," and assigns orders to specific waiters for delivery. |
| **Department (entity)** | Groups employees; owns a shared budget (e.g. department tea budget) that is separate from individual employee allowances. |
| **Department Head / In-charge** | Verifies and approves department-level consumption (e.g. tea costs) and, in later dashboard iterations, verifies generated bills using TOTP two-factor confirmation. |
| **GS Department** | Funds the monthly per-employee allowance; receives the end-of-month bill/report and pays the canteen for the allowance-covered portion of spending. |

## 4. Functional Requirements

### 4.1 Accounts & Monthly Allowance

- Each employee account is granted a monthly allowance (e.g. NPR 2,000), reset at the start of each Nepali calendar month.
- Special account types (vendor, intern, guest, board/CEO, etc.) have their own configurable limit — which may be set very high or effectively unlimited, but must be a defined, adjustable value rather than hardcoded.
- If an employee's spending exceeds their allowance, the excess is paid directly as cash at the canteen at time of pickup. This excess is treated as extra income for the canteen, not tracked against GS — no core banking system (CBS) integration is required for this in V1.
- At month end, the system calculates, per account: total spent, portion covered by the GS-funded allowance, and portion self-paid. Only the allowance-covered portion is included in the report submitted to GS Department.

### 4.2 Department Budgets (Second build priority within V1)

- Departments are a distinct entity; employees belong to a department.
- Departments can have their own shared budget for specific categories (e.g. "tea per department"), separate from individual employee allowances.
- The department head / in-charge can review and verify department-level consumption before it is finalized in the monthly report.
- **Build order note:** the individual account/allowance model is built first; department budgets are linked on top of it afterward.

### 4.3 Menu Management

- The daily menu is created and edited by the Admin (V1). Each item has a name and price.
- The menu is displayed on each consumer's dashboard for the current day.
- Future consideration (Phase 2+, not built in V1): cooks own menu decisions directly, with helper roles able to set tentative preparation times.

### 4.4 Order Lifecycle

- Consumer browses the daily menu, sees prices, and selects items to place an order.
- The order can be cancelled by the consumer at any point before Admin marks it "Processing." Once processing begins, cancellation is no longer allowed.
- After placing an order, the consumer sees a queue number and a tentative waiting time.
- Admin reviews incoming orders and assigns each one to a specific waiter for delivery.
- A waiter, once assigned, handles only their own assigned orders (not the full queue).
- When food is handed to the consumer, the order is marked "Delivered," which triggers the balance deduction from that consumer's account.
- Future consideration (Phase 2+): waiters self-editing delivery time estimates based on their own pace; cook's helpers setting tentative times.

### 4.5 Billing & Reporting

- At month end, the system aggregates each account's orders into a total, split into GS-covered and self-paid amounts.
- A report is generated and emailed/submitted to the GS Department for reimbursement of the allowance-covered total.
- Generated bills are verified by the department in-charge using Google Authenticator (TOTP) two-factor confirmation before being finalized — planned as a later dashboard feature, built after the core staff and account models are in place.

## 5. Non-Functional Requirements (Preliminary)

- **Security:** bill verification requires TOTP-based two-factor confirmation from the responsible in-charge.
- **Usability:** dashboard should clearly show remaining balance, order status, and queue position to reduce canteen-floor confusion.
- **Maintainability:** role-based structure (Admin vs. sub-staff vs. consumer) should be designed so that splitting shared logins into per-role logins later does not require a data model redesign.
- **Localization:** monthly cycle follows the Nepali calendar, not the Gregorian calendar.

## 6. Future Phases (Beyond V1)

| Phase | Feature |
|---|---|
| Phase 2 | Inventory management for canteen stock |
| Phase 2 | Dealer / vendor contact list and purchase tracking |
| Phase 2+ | Decentralized menu control (cooks manage menu directly) |
| Phase 2+ | Cook's helpers set tentative preparation/wait times |
| Phase 2+ | Waiters self-edit delivery time estimates |
| Phase 2+ | Separate logins/permissions per canteen sub-role (Order Taker, Cook, Washer, Waiter) |

## 7. Open Questions / Assumptions to Revisit

- Exact list of "special" account types and their individual limits (vendor, intern, guest, board, CEO, etc.) needs a final agreed value per type.
- Whether department budgets (e.g. tea) have their own monthly reset cycle, or draw from a separate annual/departmental budget line.
- Format and recipient list for the month-end report to GS Department (email only, or also a downloadable statement).
- Whether guests/one-off visitors need a full account or a simplified one-time order flow.

---

*End of Requirement Analysis — Draft V1.0. This document should be reviewed with the full team before moving to the Feasibility Study phase.*
