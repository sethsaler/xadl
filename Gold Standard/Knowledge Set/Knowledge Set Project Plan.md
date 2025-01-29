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

## Single-Paragraph Instruction to ChatGPT
(You can edit the bracketed parts as needed.)

[I am creating a YAML workflow for [contractual confidentiality analysis] that should (1) find tags under [“Definition & Scope of Confidentiality”], (2) define a base prompt for my lawyer persona and an analysis prompt for summarizing obligations, (3) initialize a knowledge set referencing my documents and these tags, (4) analyze each tag to retrieve relevant snippets and run them through an LLM, and (5) save the resulting summaries into an insight object and publish everything to my workspace. Please generate code that uses the tools [find-tags, echo, knowledge-set, document-text, llm, insight, workspace] with inputs in the format [function/<inputName>]—not [function:<inputName>]—and replicate the structure from my existing workflow as much as possible.]