# Knowledge Set Database Instructions

## Overview

This document provides detailed instructions for implementing and maintaining Knowledge Set YAML files. Follow these guidelines to ensure consistent and effective knowledge set creation.

## File Structure Requirements

### 1. Basic Structure
Every Knowledge Set YAML file must contain these three main sections:
```yaml
input:
  # Input declarations
actions:
  # Action definitions
functions:
  # Function definitions
```

### 2. Input Section
Required input declarations:
```yaml
input:
  documents:
    type: document
  workspace:
    type: workspace
```

### 3. Actions Section
Required action types:
- Tag Collections
- Prompts
- Knowledge Set Initialization
- Analysis Actions

## Implementation Guidelines

### 1. Tag Collection Setup

#### Required Format:
```yaml
get-[category]-tags:
  tool: find-tags
  [identifier]: [value]
```

#### Rules:
- Use descriptive category names
- Choose between `name` or `category` identifier
- Maintain consistent naming conventions

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

## Maintenance Guidelines

### 1. Version Control
- Use semantic versioning
- Document all changes
- Maintain changelog

### 2. Documentation
- Update instructions with changes
- Include examples
- Document edge cases

### 3. Testing
- Validate YAML syntax
- Test all conditions
- Verify output format

## Support and Resources

### 1. Tools
- document-text
- find-tags
- knowledge-set
- llm
- insight

### 2. Templates
- Liquid template guide
- YAML best practices
- Schema validation

### 3. Documentation
- Knowledge Set guide
- Tool documentation
- Example implementations
