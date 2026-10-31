# Feature Specification: SplitSmart Expense Sharing

**Feature Branch**: `001-expense-sharing`

**Created**: 2026-10-30

**Status**: Draft

**Input**: User description: "Read UserRequirement.md in this workspace as the authoritative customer requirements. Write a testable SplitSmart feature specification: actors, user scenarios, functional and non-functional requirements, acceptance criteria, requirement IDs (FR-001, etc.), and open questions. Preserve all original customer features in the requirements inventory; label the Java Swing MVP subset separately. Do not write Java code. Do not edit UserRequirement.md."

## Actors

- **Account Holder**: A person who creates and manages a personal account and accesses their own data.
- **Group Member**: A user who belongs to one or more groups and participates in expenses and settlements.
- **Group Manager**: A user who creates or administers a group and its membership.
- **Reviewer / Approver**: A designated user who reviews new expenses before financial impact is applied.
- **System**: The application that stores expense records, calculates balances, and sends notifications.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Create account and manage groups (Priority: P1)
A user creates a personal account, signs in, and creates or joins a group with other people who share costs. The user can manage group membership and later view group-specific financial information.

**Why this priority**: Without accounts and groups, the application cannot represent the people or shared contexts that drive all expense activity.

**Independent Test**: A new user can create an account, create a group, add members, and view the group detail without needing expense records.

**Acceptance Scenarios**:

1. **Given** a new user has no account, **When** the user creates an account and logs in, **Then** they can access their own secure account and dashboard.
2. **Given** an authenticated user, **When** they create a group and add other people to it, **Then** the group appears in their account with the correct participants.
3. **Given** a group exists, **When** the user manages the group list, **Then** the membership information reflects the latest updates.

---

### User Story 2 - Record shared expenses with split rules (Priority: P1)
A group member creates a new expense with details like amount, date, description, participants, and one of the supported split methods. The system stores the relevant monetary allocations and makes them available for balance calculations.

**Why this priority**: Expense creation is the core transactional event that drives all other settlement and reporting behavior.

**Independent Test**: A user can create an expense for a group, assign participants, and confirm that the entry is stored with the chosen split method and associated participants.

**Acceptance Scenarios**:

1. **Given** a user is part of a group, **When** they create an expense with an amount, date, description, and chosen split method, **Then** the system stores the record and associates it with the group and participants.
2. **Given** a group expense is created with an equal split, **When** the expense is saved, **Then** each participant is assigned the correct equal share as part of the expense record.
3. **Given** a group expense is created with percentage split, **When** the share percentages are recorded and saved, **Then** each participant's share matches the configured percentages.
4. **Given** an expense includes an optional receipt image, **When** the user uploads it, **Then** the receipt is attached to the expense record and remains available for review.

---

### User Story 3 - Review approval and balance adjustments (Priority: P1)
Group members can notify each other about new expenses, and expenses may go through an approval step before balances are adjusted. After approval, the system calculates each person's net balance and shows who owes whom.

**Why this priority**: Approval workflows and balance tracking are central to trust and accurate settlement among participants.

**Independent Test**: A valid expense can be approved, and the resulting balances are recalculated without manual adjustment.

**Acceptance Scenarios**:

1. **Given** a new expense requires approval, **When** the designated reviewer approves it, **Then** the system records the approval and updates balances.
2. **Given** a new expense has not yet been approved, **When** it is still pending, **Then** it is not used to adjust balances until approval is granted.
3. **Given** several users interact in a group, **When** the system recalculates balances, **Then** each person's owed and payable amounts reflect all approved transactions.
4. **Given** another group member creates a new expense, **When** notifications are triggered, **Then** the relevant users are informed of the new pending or approved expense.

---

### User Story 4 - Record payments and generate reports (Priority: P2)
A user records when money has been paid to settle balances, and the system updates the financial records. Users can review summary reports and custom date-range reports for balances, expenses, and payments.

**Why this priority**: Settlement tracking and reporting make the system useful for decision-making and reconciliation after the main expense flow works.

**Independent Test**: A user can record a payment, confirm the balance is reduced, and generate a report view of expenses and payables.

**Acceptance Scenarios**:

1. **Given** a user owes another user money, **When** a payment is recorded, **Then** the system updates the outstanding balances to reflect that settlement.
2. **Given** a user requests a report for a date range, **When** the report is generated, **Then** it includes the relevant expenses, balances, and payment activity in that period.
3. **Given** a user views their account summary, **When** they open balances and payments, **Then** they see only their relevant personal information and group activity.

---

### Edge Cases

- What happens when an expense has participants that are not part of the chosen group?
- How does the system handle a payment that exceeds the current outstanding balance?
- What happens when approval is denied or a receipt is missing or unreadable?
- How does the system behave when a user attempts to access another user's balance information without permission?
- What happens when a split percentage total does not equal 100%?

## Acceptance Criteria

- A valid account and group flow MUST allow a user to access their own dashboard and manage group membership without affecting any unrelated account.
- An expense creation flow MUST accept all required fields, support the required split methods, and persist the selected participants and receipt attachment.
- An approval flow MUST prevent unapproved expenses from affecting balances and MUST update balances only after review approval.
- A payment and reporting flow MUST reflect updated balances and produce a date-bounded summary of expenses, balances, and payments.

## Requirements Inventory

### Original Customer Requirements

- **IR-01**: User accounts must allow each person to log in and access their own expense and balance information.
- **IR-02**: Group management must support creation and management of shared groups such as roommates, trip participants, or coworkers.
- **IR-03**: Expense creation must support amount, date, description, split method, optional receipt image, and participant selection.
- **IR-04**: Notification system must inform group members when a new expense is created.
- **IR-05**: Approval workflow must support review and approval before balances are adjusted.
- **IR-06**: Balance tracking must calculate balances owed by each user to every other user.
- **IR-07**: Payment tracking must record settlements and keep balances accurate.
- **IR-08**: Reporting must provide summaries and custom reports by date range and other parameters.
- **IR-09**: Product scope must support a primary web application and also include Android and iOS apps.

### Java Swing MVP Subset

- **MVP-01**: The initial product release is a Java Swing desktop application focused on the core expense-sharing workflow for authenticated users, group management, expense entry, approval review, balance tracking, payment settlement, and report viewing.
- **MVP-02**: Web, Android, and iOS application experiences are deferred beyond the Java Swing MVP unless a later release explicitly expands scope.
- **MVP-03**: The Java Swing MVP must preserve the requirement traceability of all original customer requirements while delivering the minimal viable desktop experience.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The system MUST allow each user to create a personal account and sign in to access their own data.
- **FR-002**: The system MUST prevent users from viewing or modifying another user's personal account information outside the permitted shared-group context.
- **FR-003**: The system MUST allow users to create, edit, and manage groups containing other users.
- **FR-004**: The system MUST allow users to add or remove group participants as needed for the group lifecycle.
- **FR-005**: The system MUST allow users to create a new expense with an amount, date, description, split method, and optional receipt image.
- **FR-006**: The system MUST allow users to specify which other users are involved in an expense so the appropriate balances can be updated.
- **FR-007**: The system MUST support at least equal-split and percentage-split expense allocation methods.
- **FR-008**: The system MUST validate expense inputs so required data is present before an expense is accepted for processing.
- **FR-009**: The system MUST notify relevant group members when a new expense is created or becomes available for review.
- **FR-010**: The system MUST support an approval workflow that prevents unapproved expenses from being used to adjust balances.
- **FR-011**: The system MUST calculate balances owed between users based on approved expenses and settlements.
- **FR-012**: The system MUST allow users to record payments that settle part or all of an outstanding balance.
- **FR-013**: The system MUST update financial totals after each payment so balances remain accurate.
- **FR-014**: The system MUST provide users with expense, balance, and payment summaries for their account and groups.
- **FR-015**: The system MUST allow users to generate reports for specified date ranges and other relevant filters.
- **FR-016**: The system MUST support a Java Swing desktop MVP workflow that covers the core expense-sharing lifecycle without changing the original business requirements.
- **FR-017**: The system MUST distinguish between customer requirements and implementation decisions so design and code remain aligned with the original problem statement.

### Non-Functional Requirements

- **NFR-001**: The system MUST present account, group, and summary information clearly enough for users to understand balances without special training.
- **NFR-002**: Financial calculations MUST use an explicit decimal-based model to avoid rounding ambiguity in money values.
- **NFR-003**: The system MUST retain an auditable record of expense creation, approval status, and payment settlement for each group.
- **NFR-004**: The system MUST restrict user access so each user can only view the information required for their own account and authorized group activity.
- **NFR-005**: The system MUST be testable with scenario-based validation covering expense creation, approval, balance updates, payment settlement, and reporting.
- **NFR-006**: The system MUST be maintainable through clear separation of user interface behavior from core financial and group logic.

### Key Entities

- **User**: A person who owns a personal account and participates in one or more groups.
- **Group**: A collection of users sharing expenses, balances, and settlement activity.
- **Expense**: A financial record representing a shared transaction with amount, date, description, participants, and split logic.
- **ExpenseShare**: A per-user allocation derived from an expense and the selected splitting method.
- **ApprovalRecord**: A review outcome that determines whether an expense can affect balances.
- **Payment**: A recorded settlement reducing or clearing an outstanding balance.
- **Notification**: A message alerting users about newly created or pending expenses.
- **Report**: A summary or filtered view of expense, balance, and payment activity.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: A new user can create an account, join or create a group, record a shared expense, and view an updated balance in under 10 minutes during a standard usage test.
- **SC-002**: For a representative set of expense scenarios, 100% of expected share calculations match the configured amount, split method, and participant list.
- **SC-003**: At least 95% of users can complete a primary expense-sharing flow without needing manual support or an error correction step.
- **SC-004**: All approved expenses and recorded payments are reflected in the user-visible balances without unexplained discrepancies during reconciliation tests.
- **SC-005**: Users can generate a report for a chosen date range and confirm that it includes all relevant paid, pending, and settled activity in that timeframe.
- **SC-006**: The Java Swing MVP delivers the core workflow with no requirement drift from the original customer requirements as reviewed in the requirement traceability process.

## Assumptions

- Users are expected to have a working account and group context before creating or reviewing expenses.
- The initial version focuses on the shared-expense workflow rather than broad financial forecasting or advanced analytics.
- Group members are assumed to be trusted participants within the same household, trip, or workgroup.
- The product is expected to maintain both business logic and user-visible desk operations with clear separation between the UI and financial logic.
- The Java Swing MVP is treated as the initial delivery milestone, while web and mobile app support remain future product scope unless explicitly expanded.

## Open Questions

- **OQ-01**: Is the approval workflow expected to be a single review step or a multi-level approval process for different group types?
- **OQ-02**: Are personal expenses outside a group supported in the same product, or is the system limited to group-based costs only?
- **OQ-03**: Is the release plan intended to include a single Java Swing desktop MVP only, or should the roadmap address web and mobile parity in the same phase?
