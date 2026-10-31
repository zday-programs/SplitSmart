<!--
Sync Impact Report
- Version change: 0.0.0 -> 1.0.0
- Modified principles: generic placeholders -> Customer-Driven Requirement Traceability, Java 21 & Maven Discipline, Object-Oriented Design & Separation of Concerns, Financial Integrity with BigDecimal, Verification & Human Review
- Added sections: Technology Standards, Quality & Review Workflow
- Removed sections: none
- Follow-up TODOs: RATIFICATION_DATE pending confirmation of the original adoption date.
-->

# SplitSmart Constitution

This constitution governs SplitSmart as a Java-based expense-sharing application. The customer-facing requirements are the behaviors described in UserRequirement.md: user accounts, group management, expense creation, notifications, approval workflow, balance tracking, payment tracking, reporting, and access to account information. The technology and engineering choices below are implementation decisions selected to satisfy those requirements without changing the business intent.

## Core Principles

### I. Customer-Driven Requirement Traceability
SplitSmart must treat the requirements in UserRequirement.md as binding product obligations. Every feature, user story, test case, design artifact, and approval decision must trace to a customer requirement or a clearly documented change request. Untraced behavior is not accepted as product truth.

Rationale: requirement traceability prevents drift between customer expectations and implemented behavior, and it gives reviewers a defensible basis for approval.

### II. Java 21 and Maven Discipline
All project code and build automation MUST target Java 21 and use Maven for dependency management, reproducible builds, and release packaging. The build must be deterministic, dependency versions must be explicit, and project structure must remain consistent across modules and environments.

Rationale: a single language/runtime baseline and a repeatable build reduce integration risk and keep the project maintainable.

### III. Object-Oriented Design and Separation of Concerns
The domain model MUST use object-oriented design with explicit responsibilities for users, groups, expenses, approvals, balances, and payments. Responsibilities must be separated by concern so that business logic, validation, and calculations remain independent from persistence and presentation code.

Rationale: clear domain boundaries make the system easier to reason about, test, and extend without regressions.

### IV. Java Swing UI Integrity and GUI-Business Separation
The application user interface MUST be implemented in Java Swing, but Swing classes MUST NOT own the authoritative business rules. Validation, calculations, balance changes, and approval logic must live in domain services and model objects that are independent of UI widgets and event handlers.

Rationale: separating GUI from business logic preserves testability and prevents presentation state from corrupting financial logic.

### V. Financial Integrity and Verification Quality
All monetary values MUST use BigDecimal with explicit scale and rounding rules. Expense splits, balances, settlements, and reports must be computed from a single, auditable money model. Tests and review gates MUST verify the correct treatment of fractional cents and payment reconciliation. Human review is mandatory before AI-generated code or documentation is accepted.

Rationale: a single disciplined money model prevents hidden rounding errors and protects trust in settlement calculations.

## Technology Standards

SplitSmart MUST use Java 21, Maven, Java Swing, JUnit 5, and PlantUML as the project's governing engineering standards. JUnit 5 must cover domain logic, financial calculations, business rules, and critical user workflows. PlantUML diagrams must be used to document architecture, domain relationships, and key flows when the design changes or a requirement is newly introduced. These decisions are selected implementation choices that support the product requirements; they do not replace the customer requirements themselves.

## Quality & Review Workflow

All work must be reviewed before merge or acceptance. Code, design diagrams, tests, and requirement mappings must be checked by a human reviewer for correctness, traceability, and adherence to this constitution. AI-generated output may be used as an aid, but it is not authoritative by default and MUST be validated against the product requirements and architecture before release. Any discrepancy between a requirement and an implementation or a diagram must be resolved by explicit engineering decision and documentation.

## Governance

This constitution supersedes informal practices when there is a conflict. Amendments require a documented rationale, a review of affected requirements and design decisions, and a versioned update to this document. All changes must preserve the requirement-to-design traceability chain and must remain explicit about which items are customer requirements and which items are implementation decisions.

Compliance reviews must verify that implementation decisions remain aligned with the original customer intent and that the project continues to satisfy the governing requirements for user accounts, groups, expenses, approvals, balances, payments, and reports. A change is not considered compliant unless its business justification is traceable to the requirement set and its engineering impact is reviewed by a human stakeholder.

**Version**: 1.0.0 | **Ratified**: TODO(RATIFICATION_DATE): confirm the original adoption date for the SplitSmart constitution. | **Last Amended**: 2026-10-30
