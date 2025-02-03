# Project Plan

## Objective
Create or modify a YAML workflow that finds specific tags (e.g., “Definition & Scope of Confidentiality”), sets up prompts for LLM-based analysis, extracts relevant snippets, and saves summarized results to a knowledge set and workspace.

## Collect Tags
Use a `find-tags` action to gather all tags belonging to a chosen category (e.g., “Definition & Scope of Confidentiality”).

## Define Two Prompts
1. A `basePrompt` with your persona context (e.g., an experienced contract attorney).
2. An `analysisPrompt` describing how to summarize obligations and any relevant exceptions.

## Initialize the Knowledge Set
Use a `knowledge-set` action to create or update the knowledge set with your documents and tags.

## Analyze Each Tag
Build or reuse a function (e.g., `analyze-for-tag`) to:
- Extract document snippets tagged with the current tag (`document-text`).
- Run them through the LLM (`llm`) with your prompts.
- Create an insight object (`insight`) that captures the summary.

## Store Insights & Snippets
Add the newly created insights (and their related snippets) to your knowledge set. Optionally, publish results to a workspace (`workspace`).

## Generate Final YAML
Ensure the final YAML includes all necessary references (`function/<input>`) for each input and action, with no extraneous or unused parameters.