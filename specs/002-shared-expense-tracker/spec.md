# Feature Specification: SplitSmart Expense Sharing

**Feature Branch**: `002-shared-expense-tracker`

**Created**: 2026-10-30

**Status**: Draft

**Input**: User description: "Build SplitSmart, an expense-sharing application that allows individuals and groups to track shared expenses, payments, and balances. Read UserRequirement.md as the authoritative customer requirements. Create a testable feature specification including actors, user scenarios, functional requirements (FR-001, etc.), non-functional requirements, acceptance criteria, and open questions. Include user accounts, group management, expense creation, equal and percentage-based splitting, receipt images, notifications, approval workflows, balance tracking, payment tracking, reporting, and web/Android/iOS support. Preserve all original customer requirements. Identify the Java Swing MVP separately. Do not write Java code. Do not modify UserRequirement.md."

## Actors

- **Account Holder**: A person who creates a personal account and accesses their own balances, groups, and transaction history.
- **Group Manager**: A user who creates, edits, and manages a group and its membership.
- **Group Member**: A user who belongs to one or more groups and participates in expenses and settlements.
- **Reviewer / Approver**: A designated user who reviews a new expense before it affects balances.
- **Payer**: The user who paid the expense amount on behalf of the group or on behalf of themselves; the payer is tracked separately from the participants who share the cost.
- **System**: The application that stores financial records, performs calculations, sends notifications, and produces reports.

## Clarifications

### Session 2026-10-30
- Q: When a user creates an expense, should the payer be recorded as a separate role from the people who share the cost, or should the payer simply be treated as one of the participants whose balance is affected? → A: Payer is a separate field; the payer may or may not also be a participant in the cost split.
- Q: When a percentage-based split does not add up to 100% or leaves fractional cents, how should the system handle the invalid calculation? → A: Reject the expense if the percentages do not total 100%, and round each share using a defined cents rule before saving.
- Q: Which authentication method should the product require for a user to sign in and access their account and group data? → A: Username or email + password login for the initial product.
- Q: When a payment is recorded against an outstanding balance, should the settlement reduce the recorded debt immediately and mark the debt as settled, or should it require a separate confirmation step before the balance changes? → A: Payment reduces the outstanding balance immediately and records the settlement event.
- Q: Which members may be included in a group expense? → A: Only current group members may be included; the expense must explicitly list the participants from the selected group.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Create an account and manage a shared group (Priority: P1)
A new user creates a personal account, signs in, and creates or joins a group with other people who share costs. The user can add or remove participants and retrieve group-specific financial information.

**Why this priority**: The account and group structures define who is involved and where shared expenses are tracked.

**Independent Test**: A user can create an account, create a group, add members, and view their own and the shared group details without needing an expense record.

**Acceptance Scenarios**:

1. **Given** an unregistered user, **When** they create an account and sign in, **Then** they can access their own dashboard and group data.
2. **Given** an authenticated user, **When** they create a group and add other users, **Then** the group and membership list are stored and visible to the relevant users.
3. **Given** a group exists, **When** the group manager updates membership, **Then** the group reflects the latest member list.

---

### User Story 2 - Create and review a shared expense (Priority: P1)
A group member creates an expense with an amount, date, description, payer, list of participants, split method, and optional receipt image. The system retains the record and identifies who is responsible for what portion of the cost, while preserving the identity of the person who paid the expense.

**Why this priority**: Expense creation is the central transaction that drives financial reconciliation across the group.

**Independent Test**: A user can create an expense for a group, assign participants, select an equal or percentage split, and confirm the record is saved.

**Acceptance Scenarios**:

1. **Given** a user is a member of a group, **When** they create an expense with amount, date, description, payer, participants, and split rule, **Then** the system stores the expense with the chosen details and preserves the payer identity separately from the participants.
2. **Given** an equal-split expense, **When** it is saved, **Then** each participant's share is calculated equally according to the group participants included, and the payer retains the relevant repayment direction.
3. **Given** a percentage-split expense, **When** the percentages are saved, **Then** each participant's share is derived from the configured percentage distribution and the payer remains separately identified.
4. **Given** an expense includes a receipt image, **When** the image is uploaded, **Then** it is attached to the expense entry and remains available for review.

---

### User Story 3 - Approve expenses and maintain balances (Priority: P1)
The system notifies relevant group members when an expense is created and enforces approval before balances are adjusted. Once approved, balances reflect each user's net owed or payable position.

**Why this priority**: Trust and reconciliation depend on expenses being reviewed before they affect settlement calculations.

**Independent Test**: A valid expense can be created, reviewed, approved, and reflected in balances without manual correction.

**Acceptance Scenarios**:

1. **Given** a new expense requires approval, **When** the reviewer approves it, **Then** the system records the approval and updates balances.
2. **Given** an unapproved expense exists, **When** a user requests a balance summary, **Then** the expense is excluded from balances until approval is granted.
3. **Given** multiple users participate in a group, **When** balances are recalculated, **Then** each person's net balance reflects all approved expenses and settlements.
4. **Given** a new expense is created, **When** notification rules fire, **Then** relevant users are told that an expense is pending or approved.

---

### User Story 4 - Record payments and generate reports (Priority: P2)
A user records a payment to settle an outstanding balance. The system updates balances and provides summaries and date-bounded reports for expenses, balances, and payments.

**Why this priority**: Payment tracking and reporting turn the system from a ledger into a usable settlement and reconciliation tool.

**Independent Test**: A user can record a payment, confirm the balance changes, and generate a relevant report for a time period.

**Acceptance Scenarios**:

1. **Given** a user owes another user money, **When** they record a payment, **Then** the outstanding balance is reduced and the transaction is retained.
2. **Given** a user requests a report for a date range, **When** the report is generated, **Then** it lists the matching expenses, payments, and balances for that period.
3. **Given** a user views their dashboard, **When** they open personal balances and report summaries, **Then** they see only the information relevant to their account and authorized group activity.

---

### Edge Cases

- What happens if a user attempts to add a participant who is not in the selected group?
- What happens when a percentage total does not equal 100%?
- What happens when a payment exceeds the current outstanding balance?
- What happens when an expense is rejected or a receipt image cannot be read?
- What happens when a user tries to access another person's personal account data without permission?

## Requirements Inventory

### Original Customer Requirements

- **IR-01**: Users must have personal accounts that allow them to log in and view their own expense and balance information.
- **IR-02**: Users must be able to create and manage groups for roommates, trip participants, parties, or other shared contexts.
- **IR-03**: Users must be able to create expenses with amount, date, description, split type, optional receipt image, and involved users.
- **IR-04**: The system must notify users when group members create new expenses.
- **IR-05**: The system must include an approval workflow for reviewing and approving expenses before they affect balances.
- **IR-06**: The system must calculate and display balances owed by each user to every other user.
- **IR-07**: The system must allow users to record payments and keep balances accurate.
- **IR-08**: The system must provide reports and summaries of expenses, balances, and payments, including date-range filtering.
- **IR-09**: The product must support a primary web experience and also include Android and iOS application support.

### Java Swing MVP Subset

- **MVP-01**: The initial delivery is a Java Swing desktop application focused on the core expense-sharing workflow for accounts, groups, expense creation, approval, balances, payments, and reports.
- **MVP-02**: Web, Android, and iOS support are explicitly outside the Java Swing MVP scope unless a later release expands delivery targets.
- **MVP-03**: The Java Swing MVP must preserve the original customer requirements while delivering the smallest viable desktop product.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The system MUST allow each user to create a personal account and sign in with a standard username or email and password to access that account's own data.
- **FR-002**: The system MUST prevent a user from viewing or modifying another user's personal financial information outside the authorized group context.
- **FR-003**: The system MUST allow users to create, edit, and manage groups containing shared participants.
- **FR-004**: The system MUST allow users to add or remove group members as the group lifecycle changes.
- **FR-005**: The system MUST allow users to create a new expense with an amount, date, description, payer, involved users, and a split arrangement, and the involved users MUST be current members of the selected group.
- **FR-006**: The system MUST support equal-split expense allocation and percentage-based allocation for shared expenses, while preserving the payer identity separately from the participants.
- **FR-007**: The system MUST reject invalid percentage-based splits that do not total 100% and MUST apply a defined rounding rule when cent-level allocations are required.
- **FR-008**: The system MUST allow optional receipt image attachment for expense records.
- **FR-009**: The system MUST validate required expense fields before accepting an expense for processing.
- **FR-010**: The system MUST notify relevant group members when a new expense is created or requires review.
- **FR-011**: The system MUST support an approval workflow in which expenses are not used to adjust balances until approval is granted.
- **FR-012**: The system MUST calculate balances owed between users based on approved expenses and settlements.
- **FR-013**: The system MUST allow users to record payments that settle balances and reduce outstanding amounts.
- **FR-014**: The system MUST keep balances accurate after each payment or expense approval.
- **FR-015**: The system MUST reduce the outstanding balance immediately when a valid payment is recorded and MUST retain a settlement record for auditability.
- **FR-016**: The system MUST provide users with summaries of their expenses, payments, and current balances.
- **FR-017**: The system MUST generate reports for specified date ranges and relevant filters.
- **FR-018**: The system MUST support a Java Swing MVP that covers the core expense-sharing workflow while preserving the original product requirements.
- **FR-019**: The system MUST clearly distinguish customer requirements from implementation decisions so product scope remains traceable.

### Non-Functional Requirements

- **NFR-001**: The system MUST be understandable to users without training, so account, group, expense, and balance information remain easy to interpret.
- **NFR-002**: The system MUST use consistent financial calculations to avoid ambiguous rounding or balance discrepancies.
- **NFR-003**: The system MUST retain a clear audit trail of expense entries, approvals, payments, and report generation.
- **NFR-004**: The system MUST limit access to account and group data based on the user's authorization.
- **NFR-005**: The system MUST be testable with scenario-based validation for account creation, group management, expense entry, approval, payment handling, and reporting.
- **NFR-006**: The system MUST separate user-visible behaviors from business logic so financial rules remain maintainable and governable.

### Key Entities

- **User**: A person with an account, permissions, and participation in shared groups.
- **Group**: A collection of users sharing expenses and balances within a defined context.
- **Expense**: A transaction record containing amount, date, description, payer, participants, and split details.
- **Allocation**: A per-user share derived from an expense, the payer, and the selected splitting rule.
- **ApprovalRecord**: A review outcome that determines whether an expense can affect balances.
- **Payment**: A settlement record reducing or clearing an outstanding balance.
- **Notification**: A message or alert informing users of new or pending expense activity.
- **Report**: A filtered summary of expenses, balances, and payments over a time range.

## Acceptance Criteria

- A user can create an account, create or join a group, and view only the relevant account and group information.
- An expense can be created with the required fields, participants, and split method, and the system stores it with proof of the chosen details.
- An expense is excluded from balance calculations until the approval flow determines it is valid.
- A payment record updates the outstanding balance and is reflected in the user-visible totals.
- A report can be generated for a selected date range and includes the relevant expenses, balances, and payments.
- The Java Swing MVP delivers core expense-sharing operations without changing the original customer requirements.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: A new user can create a profile, create a group, add members, record a shared expense, and confirm the updated balance within 10 minutes in a standard usage test.
- **SC-002**: In representative expense scenarios, 100% of calculated participant shares match the configured amount, split method, and participant list.
- **SC-003**: At least 95% of test users can complete a primary expense-sharing flow without manual correction or support intervention.
- **SC-004**: All approved expenses and recorded payments appear in the balance totals without unexplained discrepancies in reconciliation checks.
- **SC-005**: Users can generate a report for a chosen period and confirm that it contains all relevant expenses, balances, and payments within that range.
- **SC-006**: The Java Swing MVP satisfies the original requirement set while keeping the desktop release explicitly scoped to the core product workflow.

## Assumptions

- Users are expected to participate in a group or otherwise share expenses with defined peers.
- The product treats expense approval as a required review stage before balance adjustments take effect.
- The initial scope covers core tracking and reporting rather than advanced forecasting or analytics.
- Payment and report flows are expected to operate on the same underlying financial record set for consistent balances.
- The Java Swing MVP is a first release milestone, while web and mobile support remain future product scope unless explicitly expanded.

## Open Questions

- **OQ-01**: Should the approval workflow be a single-step review or a multi-level approval process depending on group type or transaction size?
- **OQ-02**: Is personal expense tracking outside a group in scope, or is the system limited to group-based shared costs only?
- **OQ-03**: Is the initial release limited to Java Swing desktop only, or should the roadmap include a parallel web/mobile delivery plan in the same phase?
