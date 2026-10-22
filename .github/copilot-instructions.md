# GitHub Copilot & OpenAI Codex Instructions

This project provides an automated AI agent skillset for creating and managing a Software Project Knowledge Base in Notion.

## Repository Knowledge Structure
- `.agents/skills/`: Contains modular skill instructions with detailed workflows:
  - `notion-project-init`: Bootstrap root page and Database 1 with full SQL DDL schema.
  - `knowledge-base-manager`: Central master skill orchestrating all 5 knowledge base sections.
  - `requirements-normalizer`: Raw requirements -> atomic normalized REQ-* items without hallucinations.
  - `requirements-analyzer`: Quality audit. Approves solid requirements; logs issues in Section 2 table.
  - `requirements-resolver`: Generates human questionnaires in `clarifications/` (strictly outside Git).
  - `requirements-decision-applier`: Parses human responses from `.md` files and updates Notion statuses.
  - `openapi-spec-generator`: Generates OpenAPI 3.0+ specs and endpoint matrix into Section 5.

## Core Rules:
1. **Preserve Intent**: Never modify business logic or invent constraints during normalization.
2. **Never Decide for the User**: When identifying defects, ask open questions and offer options, but never pick an option autonomously.
3. **Folder Isolation**: All review markdown files must be kept inside `clarifications/` which is ignored by `.gitignore`.
4. **Language**: All output, requirements, documentation, and user communication must be in **Ukrainian**.
