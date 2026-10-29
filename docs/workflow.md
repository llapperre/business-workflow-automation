# Order processing workflow

The production workflow supports multiple order sources and a shared processing path. The following is a simplified representative flow rather than a specification of every branch.

## 1. Collect the order

Orders may be received via email, read from a customer spreadsheet, or collected from a customer portal using Playwright. Each adapter extracts relevant details such as order identifier, target site, article source, and image information.

## 2. Normalize and avoid duplicates

Source-specific values are transformed into shared order/item records. The workflow checks whether an item has already been recorded or has an existing resource folder, helping avoid duplicate processing.

## 3. Save and validate resources

Documents, images, and useful source artifacts are stored in an organized Google Drive structure. Required data is checked before the item proceeds.

## 4. Prepare the article

The content-processing stage cleans HTML and prepares publication-ready markup, including appropriate headings, links, and paragraph structure. Inconsistent incoming document formats make this step important.

## 5. Prepare the image

Customer-supplied images may be used when suitable. The workflow also supports finding and processing alternative images and can leave the final selection to an operator.

## 6. Create a WordPress post

An integration bridge supports creating a draft or publishing, according to the execution profile and configured side-effect controls. Media and category handling are coordinated with the target site.

## 7. Record the outcome

The tracking spreadsheet is updated with status, resources, and draft or published URL where applicable. Failures and items that need manual attention remain visible.

## 8. Communicate results

Notifications can summarize the outcome of individual orders and the overall processing run. Customer communication and exceptional decisions are still handled by a person when needed.

## Status and operational controls

The process uses states such as **Received**, **Validated**, **Ready**, **Drafted**, **Published**, and **Fix Needed**. Not every item reaches publication in a single run, and the process distinguishes drafts from live posts.

Test/live execution profiles limit unwanted external effects, for example by controlling WordPress publication, spreadsheet writebacks, and marking emails as read.

## Human-in-the-loop design

The goal is not to eliminate oversight. It is to minimize mechanical tasks while maintaining human control over exceptional content, images, and customer relationships.
