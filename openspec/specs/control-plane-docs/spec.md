# control-plane-docs Specification

## Purpose
Define how high-weight control-plane documents stay durable, authoritative, and free from transient project-state pollution.
## Requirements
### Requirement: Stable control-plane documents exclude transient project state
High-weight control-plane documents MUST record durable rules, boundaries, contracts, acceptance criteria, and workflow constraints. They MUST NOT become a running log of temporary status, weekly plans, dated milestones, or short-lived ownership assignments.

#### Scenario: Writing a governance document
- **WHEN** an author updates a stable governance, workflow, or control-plane document
- **THEN** the document MUST prioritize durable constraints over iteration-specific status

#### Scenario: Detecting transient content in a high-weight document
- **WHEN** a document contains concrete weekly status, temporary owners, issue numbers, or dated progress notes
- **THEN** that content MUST be removed or moved to a lower-weight sync record

### Requirement: Transient operational state is externalized from stable rule documents
Operational state that changes frequently MUST live in copied templates, sync records, release notes, or other low-weight artifacts instead of the main body of stable rule documents.

#### Scenario: Team needs a current status snapshot
- **WHEN** the team needs to record current progress, blockers, candidate release state, or next actions
- **THEN** the information MUST be placed in a sync record or runtime dashboard artifact rather than the stable rule source

### Requirement: Control-plane documents define authoritative boundaries
Control-plane documents MUST identify which repositories, templates, and rules are authoritative for governance, release, and validation decisions.

#### Scenario: Agent chooses a source of truth
- **WHEN** an agent or contributor needs to decide which material has highest authority
- **THEN** the control-plane document set MUST point to the stable source of truth for that decision domain
