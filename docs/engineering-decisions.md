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

## Treat AI-generated articles as another intake source

**Constraint:** Articles may originate from customer-supplied documents or from an AI generation workflow.

**Approach:** Read topics from a sheet, assemble generation instructions according to multiple criteria, save generated articles to a dedicated spreadsheet, and let the publishing pipeline ingest that sheet as another source.

**Trade-off:** Generation and publishing remain separate stages with an explicit handoff, allowing each to be tracked and reasoned about independently.

## Discover videos from relevant YouTube sources

**Approach:** First search for suitable videos on a client's YouTube channel; when no client channel is available, search a curated set of trusted channels.

**QA rationale:** Source selection matters as well as topical relevance; the trusted-channel fallback avoids treating arbitrary YouTube results as equivalent.

## Handle varied source documents and imagery

**Constraint:** Customer content is not consistently formatted, and images may come from different sources.

**Approach:** Normalize HTML, guard against lost links, validate image suitability, and enrich articles with relevant videos where available.

**QA rationale:** It is safer to surface problematic input than to publish an apparently successful but damaged article.

## Use Google Sheets instead of a database

**Context:** The tracking data is not only consumed by code. People need to read order details, inspect statuses, filter records, and occasionally make operational changes without using developer tools.

**Decision:** Use Google Sheets as the shared operational tracking interface instead of introducing a separate database and building an additional interface for non-technical users.

**Benefits:** The team can access the information in a familiar format, with straightforward visibility into processing results and exceptions. This reduces the need for a custom admin interface.

**Trade-offs:** Sheets is not a relational database. API quotas, concurrent updates, data validation, and consistency need explicit attention. This choice fits the current business scale and workflow; it would need reassessment if volume, concurrency, or reporting requirements grew substantially.

## Hosted runtime and browser compatibility

**Decision:** Run the Python automation in a hosted PythonAnywhere environment rather than relying on an operator's local computer.

**Operational challenge:** Playwright's browser runtime required attention to Chromium compatibility and launch configuration in that environment.

**Trade-off:** Hosting supports repeatable execution but adds environment-specific dependencies that need to be tested and maintained.

## Work incrementally with production constraints

External services introduce rate limits, unavailable data, changing interfaces, and differences between draft creation and public publishing. The workflow evolved through testing, debugging, and practical operational feedback. Its normal unattended execution reaches WordPress draft creation; optional manual order entry and later operator activity are outside that requirement.

The case study deliberately focuses on system boundaries, risk controls, and decisions rather than publishing sensitive source files.

## What I would continue improving

Areas for further work include strengthening automated checks around external integration points, simplifying state transitions, expanding observability, and reducing UI automation dependencies where stable APIs become available. These are directions for improvement, not claims that they are already implemented.
