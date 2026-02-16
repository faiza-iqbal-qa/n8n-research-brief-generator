n8n Research Brief Generator – Iteration 2

This project started as a simple experiment:

Can a Google Sheet act as a lightweight AI research engine?

Version 1 proved it works.
Version 2 focuses on reliability and system behavior.

What It Does

Add a URL to a Google Sheet and the workflow:

Fetches the article via HTTP

Generates a structured brief using an LLM

Writes results back into separate columns

Tracks row state (PROCESSING → DONE / FAILED)

Prevents duplicate processing

Increments attempt counters

What Improved in This Iteration

Clear success vs failure branching

Outcome-based validation (checks for structured output instead of relying on error flags)

Stable row matching using row_number

Explicit Status column (PROCESSING / DONE / FAILED)

Controlled batch size (1 row at a time)

This version focuses less on prompting and more on workflow behavior.

Workflow Overview

Schedule Trigger
→ Get rows from Google Sheets
→ Filter unprocessed rows
→ Loop Over Items (batch size = 1)
→ Mark row as PROCESSING
→ HTTP Request
→ Transform content
→ Message a Model
→ IF (validate structured output)

If success:
→ Update sheet with structured brief
→ Mark DONE + Processed = YES

If failure:
→ Mark FAILED
→ Log error
→ Increment attempts

Required Google Sheet Columns

row_number

URL

Executive Summary

Key Themes

Strategic Risks

Opportunities

LinkedIn Angles

Processed

Status

Attempts

Last run

Error

Tech Stack

n8n (Cloud)

Google Sheets

HTTP Request Node

LLM via Message a Model

JavaScript transformation nodes

How To Run

Import workflow JSON into n8n

Create Google Sheet using sheet-template.csv

Connect credentials

Execute workflow

Add URLs to test

Built while iterating and learning in public.
