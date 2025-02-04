# Knowledge Set Instructions

## Overview

This document provides instructions for creating Knowledge Sets focused on analyzing credit-related clauses in talent agreements. Follow these guidelines to ensure consistent and effective legal analysis.

## Requirements

1. Create the YAML with document and workspace input types.
2. The actions will need to get and use tags from two sources:
   - Agreement Information tags ("Credit", "Preamble")
   - Document Category tags from "Document Tags" category
3. Create the appropriate actions for:
   - Setting up base prompts for legal analysis
   - Configuring contract analysis prompts
   - Building functions to analyze credit-related content

## Action Type Structure Requirements

1. All analysis actions MUST use the following structure:
```yaml
analyze-[category]:
  repeat:
    function: analyze-for-tag
    for: [tag]
  input:
    tag: get-[category]-tags
    documents: task/documents
    knowledge: init-kset
    basePrompt: basePrompt
    prompt: contractAnalysisPrompt
    title: contractAnalysisTitle
```

2. Key points about the structure:
   - Always use `repeat` with `function` and `for` parameters
   - The function name must be `analyze-for-tag`
   - The `for` parameter must be `[tag]`
   - Never use direct function calls with `function: analyze-for-category`
   - Keep all input parameters under the `input` section

## Implementation Guidelines

### 1. Base Prompt Setup
- Establish the role as in-house counsel
- Define clear objectives for credit analysis
- Include guidelines for maintaining legal precision

### 2. Contract Analysis Prompt
- Build on base prompt
- Focus on credit obligations and risks
- Include specific review criteria

### 3. Tag Collection Setup
```yaml
get-agreementInformation-tags:
  tool: find-tags
  name: ["Credit", "Preamble"]

get-documentCategory-tags:
  tool: find-tags
  category: "Document Tags"
```

## Common Issues to Avoid

1. ❌ Incorrect structure (Do not use):
```yaml
analyze-[category]:
  input:
    tag: get-[category]-tags
    documents: task/documents
    knowledge: init-kset
  function: analyze-for-category  # Wrong! Use repeat structure instead
```

2. ✅ Correct structure:
```yaml
analyze-[category]:
  repeat:
    function: analyze-for-tag
    for: [tag]
  input:
    tag: get-[category]-tags
    documents: task/documents
    knowledge: init-kset
    basePrompt: basePrompt
    prompt: contractAnalysisPrompt
    title: contractAnalysisTitle
```

### 2. Prompt Configuration

#### Base Prompt Format:
```yaml
basePrompt:
  tool: echo
  message: |
    # [Context Title]
    [Role description]
    [Objectives]
    [Guidelines]
```

#### Analysis Prompt Format:
```yaml
analysisPrompt:
  tool: echo
  message: |
    {{basePrompt}}
    [Additional context]
```

### 3. Knowledge Set Initialization

#### Required Format:
```yaml
init-kset:
  input:
    documents: task/documents
    [tagCollection]: get-[category]-tags
  tool: knowledge-set
  knowledgeSetName: "[Name]"
  operations:
    - op: add-dimension
      key: [key]
      name: [name]
      nodes: [nodes]
      priority: [number]
```

#### Rules:
- Include all tag collections
- Use meaningful dimension names
- Assign appropriate priorities

### 4. Analysis Actions

#### Required Format:
```yaml
analyze-[category]:
  repeat:
    function: analyze-for-tag
    for: [tag]
  input:
    tag: get-[category]-tags
    documents: task/documents
    knowledge: init-kset
    basePrompt: basePrompt
    prompt: analysisPrompt
    title: analysisTitle
```

#### Rules:
- Always use the repeat structure
- Include all required input parameters
- Maintain consistent naming

## Function Implementation

### 1. Basic Function Structure
```yaml
[function-name]:
  input:
    [parameter]:
      type: [type]
  actions:
    [action-name]:
      input: [input-params]
      tool: [tool-name]
```

### 2. Required Function Components

#### Text Extraction:
```yaml
get-snippets:
  input:
    tag: function/tag
    documents: function/documents
  tool: document-text
  textFilter:
    tags: [tag]
```

#### Instruction Prompts:
```yaml
get-instruction-prompt-for-tag:
  input:
    tag: function/tag
  tool: echo
  message: |
    {% if [condition] %}
    [Format requirements]
    {% endif %}
```

#### Content Analysis:
```yaml
analyze-content:
  input:
    snippets: get-snippets
    basePrompt: function/basePrompt
    instructionPrompt: get-instruction-prompt-for-tag
  tool: llm
  skipIfEmpty: snippets
```

## Best Practices

### 1. Naming Conventions
- Use kebab-case for dimension keys
- Use PascalCase for knowledge set names
- Use snake_case for function names
- Use descriptive prefixes for actions

### 2. Type Declarations
- Always specify types for input parameters
- Use appropriate type constraints
- Document custom types

### 3. Skip Conditions
- Use skipIfEmpty for optional processing
- Specify clear skip conditions
- Handle empty cases gracefully

### 4. Template Usage
- Use Liquid syntax for logic
- Maintain consistent formatting
- Document template variables

## Common Issues and Solutions

### 1. Structure Issues
❌ Incorrect:
```yaml
analyze-[category]:
  function: analyze-for-category
```

✅ Correct:
```yaml
analyze-[category]:
  repeat:
    function: analyze-for-tag
    for: [tag]
```

### 2. Type Issues
❌ Incorrect:
```yaml
input:
  tag: string
```

✅ Correct:
```yaml
input:
  tag:
    type: tag
```

### 3. Skip Condition Issues
❌ Incorrect:
```yaml
skipIfEmpty: true
```

✅ Correct:
```yaml
skipIfEmpty: snippets
```