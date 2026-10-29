# Order processing workflow

The production workflow supports multiple order sources and a shared processing path. The following is a simplified representative flow rather than a specification of every branch.

## 1. Collect the order

Orders may be received via email, read from a customer spreadsheet, collected from a customer portal using Playwright, or taken from a generated-articles spreadsheet. The AI content source reads article topics from a sheet, combines criteria-based instructions, generates articles, and writes completed articles to the sheet consumed by the publishing pipeline. Manual order additions are also supported. Each adapter extracts relevant details such as order identifier, target site, article source, and image information.

## 2. Normalize and avoid duplicates

Source-specific values are transformed into shared order/item records. The workflow checks whether an item has already been recorded or has an existing resource folder, helping avoid duplicate processing.

## 3. Save and validate resources

Documents, images, and useful source artifacts are stored in an organized Google Drive structure. Required data is checked before the item proceeds.

## 4. Prepare the article

The content-processing stage cleans HTML and prepares publication-ready markup, including appropriate headings, links, and paragraph structure. Inconsistent incoming document formats make this step important.

## 5. Prepare the image

Customer-supplied images may be used when suitable. The workflow also supports finding and processing alternative images.

## 6. Discover a relevant YouTube video

The video workflow checks whether the client has a YouTube channel and searches it for suitable content. When no client channel is available, it searches a curated set of trusted channels.

## 7. Create a WordPress draft

An integration bridge creates a WordPress draft, including associated media and category handling, according to the execution profile and side-effect controls. Media and category handling are coordinated with the target site.

## 8. Record the outcome

The tracking spreadsheet is updated with status, resources, and draft or published URL where applicable. Failures and items that need manual attention remain visible.

## 9. Communicate results

Notifications can summarize the outcome of individual orders and the overall processing run. Customer communication and any subsequent editorial decisions are separate from the unattended pipeline.

## Status and operational controls

The process uses states such as **Received**, **Validated**, **Ready**, **Drafted**, **Published**, and **Fix Needed**. Not every item reaches publication in a single run, and the process distinguishes drafts from live posts.

Test/live execution profiles limit unwanted external effects, for example by controlling WordPress publication, spreadsheet writebacks, and marking emails as read.

## Automation boundary

The pipeline can run without human intervention from intake through WordPress draft creation. Operators can add orders manually, and people may separately review drafts or communicate with clients. Those activities do not make manual intervention a prerequisite for a normal run.
