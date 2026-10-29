# Engineering decisions and QA approach

These notes describe the reasoning behind selected design choices. They are not a claim that the system is a general-purpose publishing platform or a fully autonomous solution.

## Use browser automation where an API is missing

**Constraint:** One order source was exposed through a customer portal rather than a suitable direct API.

**Approach:** Use Playwright to access the relevant workflow and extract order details.

**Trade-off:** This enabled integration without changing the upstream system, but UI automation is sensitive to markup changes, login state, and browser/runtime compatibility.

## Check for duplicates before processing

**Constraint:** Repeated runs or source reads can encounter items that were already processed.

**Approach:** Use order/item identifiers and existing stored resources to avoid creating duplicate work.

**Trade-off:** Identity rules must account for orders containing multiple items; a simple order ID alone is not always sufficient.

## Keep test and live activity separated

**Constraint:** The system can trigger real side effects — publishing posts, updating client spreadsheets, and changing email read status.

**Approach:** Use explicit runtime profiles and controls for side effects; distinguish test resources and tracking from production resources.

**QA rationale:** Testing an integration should not silently modify a customer's real workflow. Observability and the ability to disable specific writes matter as much as the functional path.

## Preserve traceability

**Constraint:** Orders can fail for different reasons at different stages, and an operator needs to know what was completed.

**Approach:** Track states, saved assets, resulting WordPress links, failure reasons, and run-level summaries.

**Trade-off:** State transitions and external writebacks require discipline to prevent inconsistent or misleading status.

## Handle varied source documents and imagery

**Constraint:** Customer content is not consistently formatted, and images may come from different sources.

**Approach:** Normalize HTML, guard against lost links, validate image suitability, and support manual image selection when needed.

**QA rationale:** It is safer to surface problematic input than to publish an apparently successful but damaged article.

## Work incrementally with production constraints

External services introduce rate limits, unavailable data, changing interfaces, and differences between draft creation and public publishing. The workflow evolved through testing, debugging, and practical operational feedback.

The case study deliberately focuses on system boundaries, risk controls, and decisions rather than publishing sensitive source files.

## What I would continue improving

Areas for further work include strengthening automated checks around external integration points, simplifying state transitions, expanding observability, and reducing UI automation dependencies where stable APIs become available. These are directions for improvement, not claims that they are already implemented.
