# AI Research Brief Generator (n8n + Claude + Google Sheets)

## What this does
Paste article URLs into a Google Sheet. The workflow fetches each URL, generates a structured brief using Claude, and writes the output back into the sheet (Executive Summary, Themes, Risks, Opportunities, LinkedIn angles). It also marks rows as Processed so it doesn’t re-run on the same URLs.

## Input (Google Sheet columns)
Create a Google Sheet with these headers in row 1:

- URL
- Executive Summary
- Key Themes
- Strategic Risks
- Opportunities
- LinkedIn Angles
- Processed

Add URLs under the URL column. Leave Processed blank.

## Output
For each URL, the workflow fills:
- Executive Summary
- Key Themes
- Strategic Risks
- Opportunities
- LinkedIn Angles
- Processed = YES

## Workflow logic (high level)
1. Schedule Trigger runs on an interval (ex: every 15 minutes)
2. Google Sheets: Get Row(s)
3. IF: only continue when Processed is empty
4. Loop Over Items (batch size 1): process rows one at a time
5. HTTP Request: fetch article HTML
6. Edit Fields: create `articleText` by trimming the HTML to a safe length
7. AI (Claude): generate a structured brief with fixed headings
8. Code node: split the model output into separate sections
9. Google Sheets: Update Row by matching on `row_number` and writing results back

## Setup
### 1) Google Sheets
- Create the sheet with the columns above.
- Add a few URLs in the URL column.

### 2) n8n credentials
- Google Sheets credential (OAuth)
- Anthropic / Claude API key credential

### 3) Important note on cost control
The workflow only calls Claude for rows where Processed is blank. Once a row is updated, Processed becomes YES and the row is skipped in future runs.

## Known limitations
- Some websites block automated requests (403) or return paywalled content.
- V1 uses raw HTML; results improve if you add better text extraction later.

## Next improvements
- Cleaner article extraction (remove navigation/footer)
- Better error logging + retries
- Deduplicate repeated URLs

## How to Run This

1. Import `workflow.json` into n8n
2. Create a Google Sheet using `sheet_template.csv`
3. Connect Google Sheets credential in n8n
4. Connect Anthropic (Claude) API credential
5. Set Schedule Trigger (e.g., every 15 minutes)
6. Add URLs into the sheet
7. Activate the workflow

Rows with blank `Processed` will be summarized and marked as `YES
