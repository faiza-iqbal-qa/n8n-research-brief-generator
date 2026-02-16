n8n Research Brief Generator (Iteration 2)

This project started as a simple experiment:

Can a Google Sheet become a lightweight AI research engine?

Version 1 proved it could.

Version 2 focuses on making it behave like a system, not a demo.

What It Does

Add a URL to a Google Sheet and the workflow:

Fetches the article via HTTP

Generates a structured brief using an LLM

Writes results back into the sheet

Tracks row state (PROCESSING → DONE / FAILED)

Prevents duplicate processing

Increments attempt counters

Handles invalid URLs gracefully

What Changed in v2

This iteration improves reliability and structure:

Clear success vs failure branching

Outcome-based validation (checks if structured output exists)

Proper row_number matching for stable updates

Attempts tracking

Explicit Status column (PROCESSING / DONE / FAILED)

Controlled batch size (1 row at a time)

The goal was not better prompting.

The goal was better system behavior.

Workflow Architecture

Schedule Trigger
→ Get rows from Google Sheets
→ Filter unprocessed rows
→ Loop Over Items (batch size = 1)
→ Mark row as PROCESSING
→ HTTP Request (fetch article)
→ Clean/transform content
→ Message a Model (LLM)
→ IF (check if structured output exists)

True branch (Success):
→ Update sheet with structured brief
→ Mark DONE + Processed = YES

False branch (Failure):
→ Update sheet with FAILED status
→ Log error
→ Increment attempts

Google Sheet Structure

The sheet must contain the following columns:

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

A template file is included:

sheet-template.csv

Tech Stack

n8n (Cloud)

Google Sheets

HTTP Request Node

LLM via “Message a Model”

JavaScript transformation node

How To Run

Import the workflow JSON into n8n

Create a Google Sheet using sheet-template.csv

Connect Google Sheets credentials

Connect your LLM provider credentials

Execute workflow

Add URLs to test

What This Demonstrates

Practical AI workflow automation

State-driven design

Idempotent processing

Failure-safe branching

Iterative system improvement

This repository reflects the second iteration after identifying weaknesses in the original design.

Still refining. Still learning.