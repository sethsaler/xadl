# DiffChecker v2 Documentation

## Overview
DiffChecker_v2 is an automation framework designed to analyze and compare documents across multiple predefined categories. It processes each document-category pair individually and then performs pairwise comparisons between documents for each category.

## Structure

### 1. Input Configuration
```yaml
input:
  documents:
    type: document
  workspace:
    type: workspace
```

### 2. Core Components

#### Tag Collections
- Separate tag collections for each category (e.g., Performance, Payment, SLA)
- Uses `find-tags` tool with category filters
```yaml
get-performance-tags:
  tool: find-tags
  category: "Performance Obligations"
```

#### Knowledge Set Initialization
- Creates dimensions for each category
- Sets document nodes and tag relationships
- Priority ordering for dimensions

#### Analysis Actions
- One analysis action per category
- Uses repeat function to process each document-tag pair
- Creates insights for individual analyses

#### Comparison Actions
- One comparison action per category
- Performs pairwise document comparisons
- Creates comparison matrix insights

## How to Recreate

1. **Define Categories**
   - Identify your tag categories
   - Create corresponding tag collection actions

2. **Create Analysis Actions**
```yaml
analyze-category:
  repeat:
    function: analyze-category-for-document
    for: [documentTag, tag]
  input:
    documentTag: get-document-tags
    tag: get-category-tags
    documents: task/documents
    knowledge: init-kset
```

3. **Create Comparison Actions**
```yaml
compare-documents-category:
  repeat:
    function: compare-documents-for-category
    for: [tag]
  input:
    tag: get-category-tags
    knowledge: init-kset
    analyze-categories: analyze-category
```

## Creating New Versions with Different Categories

### Instructions for LLMs

To create a new version with different categories:

1. **Tag Collection Template**
```yaml
get-[category]-tags:
  tool: find-tags
  category: "[Category Name]"
```

2. **Knowledge Set Dimension Template**
```yaml
- op: add-dimension
  key: [category-key]
  name: "[Category Name]"
  nodes: [category]Tags
  priority: [number]
```

3. **Analysis Action Template**
```yaml
analyze-[category]:
  repeat:
    function: analyze-category-for-document
    for: [documentTag, tag]
  input:
    documentTag: get-document-tags
    tag: get-[category]-tags
    documents: task/documents
    knowledge: init-kset
```

4. **Comparison Action Template**
```yaml
compare-documents-[category]:
  repeat:
    function: compare-documents-for-category
    for: [tag]
  input:
    tag: get-[category]-tags
    knowledge: init-kset
    analyze-categories: analyze-[category]
```

### Steps for Creating New Versions

1. Replace category names in tag collections
2. Update knowledge set dimensions
3. Create corresponding analysis actions
4. Create corresponding comparison actions
5. Update workspace additions

### Example Instruction for LLM

"Create a version of DiffChecker_v2 using these categories:
- Category1
- Category2
- Category3

Follow the templates above and maintain the same structure and relationships between components."

## Best Practices

1. **Naming Conventions**
   - Use lowercase, hyphenated names for actions
   - Keep category names consistent throughout
   - Use descriptive insight names

2. **Dependencies**
   - Maintain clear dependency chains
   - Add explicit dependencies in workspace action
   - Keep analysis and comparison pairs linked

3. **Knowledge Set Structure**
   - Maintain consistent dimension priorities
   - Use clear dimension keys
   - Keep document dimension as priority 1

4. **Comparison Logic**
   - Maintain pairwise comparison structure
   - Keep comparison categories consistent
   - Use clear delimiter structure

## Common Issues and Solutions

1. **Dimension Matching**
   - Use tags instead of dimension matching when possible
   - Keep dimension keys simple and consistent
   - Match on document dimension first

2. **Dependency Chains**
   - Ensure each comparison depends on its analysis
   - Keep workspace action updated with all components
   - Maintain clear input/output relationships

3. **Tag Categories**
   - Use exact category names in find-tags
   - Keep category names consistent
   - Use quotes for multi-word categories

## Extending the Framework

To add new functionality:
1. Add new tag collections
2. Create corresponding dimensions
3. Add analysis actions
4. Add comparison actions
5. Update workspace additions
6. Maintain dependency relationships