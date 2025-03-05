# Prompt Generator for Creating XADL Comparison Files

Based on my analysis of your codebase, particularly the High-Level Comparison.yml and Risk Management.yml files, here's a detailed prompt that will help you create similar files in the future:

## Complete Prompt for Sub-LLM

```
# Task: Create a XADL Comparison File

## Context
You are tasked with creating a YAML file for document comparison analysis in the XADL framework. This file will define a workflow that analyzes and compares documents across different categories or dimensions.

## Requirements

1. Create a complete YAML file that follows the XADL structure for document comparison
2. The file MUST use `set-nodes` operations instead of `add-items` in all knowledge-set operations
3. Follow the standard input structure with documents and workspace types
4. Implement a multi-step workflow that:
   - Collects document and category tags
   - Sets up base prompts for analysis
   - Creates a knowledge set
   - Analyzes content for each category-document pair
   - Compares documents across categories
   - Adds results to the workspace

## Step-by-Step Instructions

### Step 1: Define the Input Section
Create the input section with document and workspace types. Example:
```yaml
input:
  documents:
    type: document
  workspace:
    type: workspace
```

PAUSE HERE FOR FEEDBACK

### Step 2: Define Initial Actions for Tag Collection
Create actions to collect document tags and category tags. Example:
```yaml
actions:
  # 1) Tag Collections for Different Categories
  get-document-tags:
    tool: find-tags
    name: ["Document1", "Document2", "Document3"]

  get-category-tags:
    tool: find-tags
    category: "Category"
```

PAUSE HERE FOR FEEDBACK

### Step 3: Define Base Prompts
Create base prompts that will be used in the analysis. Example:
```yaml
  # 2) Base configuration for shared prompt elements
  basePrompt:
    tool: echo
    message: |
      # Role and Context Setup
      You are a document analysis expert focused on summarizing and comparing document content across categories.
      
      [Include detailed instructions for the analysis]
```

PAUSE HERE FOR FEEDBACK

### Step 4: Define Analysis Prompts
Create specific analysis prompts that build on the base prompt. Example:
```yaml
  # 3) Analysis prompt
  categoryAnalysisPrompt:
    tool: echo
    message: |
      {{basePrompt}}

      Below is relevant text from the document sections. Please:
      - Analyze the specific category terms and their usage
      - Compare terminology across document sections
      [Include additional analysis instructions]
```

PAUSE HERE FOR FEEDBACK

### Step 5: Initialize Knowledge Set
Create a knowledge set initialization action using `set-nodes` instead of `add-items`. Example:
```yaml
  # 5) Initialize Knowledge Set
  init-kset:
    input:
      documents: task/documents
      documentTags: get-document-tags
      categoryTags: get-category-tags
    tool: knowledge-set
    knowledgeSetName: "Document Category Analysis and Comparison"
    operations:
      - op: set-nodes
        nodes: documents
      - op: add-dimension
        key: document
        name: "Document"
        nodes: documentTags
        priority: 1
        skipIfEmpty: true
      - op: add-dimension
        key: category
        name: "Category"
        nodes: categoryTags
        priority: 2
        skipIfEmpty: true
```

PAUSE HERE FOR FEEDBACK

### Step 6: Define Analysis Functions
Create functions to analyze categories for each document. Example:
```yaml
functions:
  analyze-category-for-document:
    input:
      documentTag:
        type: tag
      categoryTag:
        type: tag
      [Include other input parameters]
    actions:
      [Define actions for analysis]
      
      add-insight:
        input:
          knowledge: function/knowledge
          insight: create-category-insight
          snippets: get-snippets
        tool: knowledge-set
        operations:
          - op: set-nodes
            nodes: insight
          - op: set-nodes
            nodes: snippets
```

PAUSE HERE FOR FEEDBACK

### Step 7: Define Comparison Functions
Create functions to compare documents for each category. Example:
```yaml
  compare-documents-for-category:
    input:
      categoryTag:
        type: tag
      knowledge:
        type: knowledge-set
    actions:
      [Define actions for comparison]
      
      add-comparison:
        input:
          knowledge: function/knowledge
          insight: create-comparison-insight
        tool: knowledge-set
        operations:
          - op: set-nodes
            nodes: insight
```

PAUSE HERE FOR FEEDBACK

### Step 8: Define Workspace Integration
Create actions to add results to the workspace using `set-nodes` instead of `add-items`. Example:
```yaml
  # 8) Add Results to Workspace
  add-to-workspace:
    input:
      workspace: task/workspace
      categoryAnalysis: analyze-categories
      documentComparisons: compare-documents
      knowledge: init-kset
    tool: workspace
    operations:
      - op: set-nodes
        items: knowledge
```

PAUSE HERE FOR FEEDBACK

### Step 9: Review and Finalize
Review the entire YAML file to ensure:
1. All knowledge-set operations use `set-nodes` instead of `add-items`
2. All required components are included
3. The workflow logic is complete and coherent
4. The file follows YAML syntax correctly

## Important Notes
- ALWAYS use `set-nodes` instead of `add-items` in knowledge-set operations
- Ensure proper indentation in the YAML file
- Include appropriate comments to explain each section
- Make sure all references to other actions or functions are correct
- Test the file with sample documents to verify functionality
```

## Additional Guidance for Creating Effective Prompts

When using this prompt generator, consider these additional tips:

1. **Customize the document and category tags**: Modify the `get-document-tags` and `get-category-tags` sections to match your specific use case.

2. **Tailor the analysis prompts**: The base prompts and analysis prompts should be customized to the specific type of comparison you're trying to achieve.

3. **Adjust the knowledge set structure**: Modify the dimensions and priorities in the knowledge set initialization based on your specific analysis needs.

4. **Emphasize the use of `set-nodes`**: Make it clear that all knowledge-set operations must use `set-nodes` instead of `add-items`.

5. **Break down complex tasks**: The step-by-step approach with pauses for feedback ensures the sub-LLM builds the solution incrementally and stays focused.