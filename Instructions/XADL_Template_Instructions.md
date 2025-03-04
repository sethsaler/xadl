# XADL Template Instructions

To create a new YAML automation document for the XADL codebase, use the following template and replace the placeholders with your specific implementation details. This template provides the basic structure for creating automations that process documents, apply tags, and generate analysis.

```yaml
input:
  # Define your input sources here
  documents:
    type: document  # Use this to process document files
  workspace:
    type: workspace  # Access the workspace environment
  customInput:
    type: text  # Use text instead of string as per requirements
    label: "{{LABEL_TEXT}}"
    defaultValue: "{{DEFAULT_VALUE}}"
   
actions:
  # Define your primary actions here
  find-relevant-tags:
    tool: find-tags
    name: "{{TAG_NAME}}"  # Replace with your tag name or reference customInput
    
  process-documents:
    input:
      documents: task/documents
      tags: find-relevant-tags
    tool: document-text
    textFilter:
      tags: [tags]
      
  generate-analysis:
    input:
      processedData: process-documents
    tool: llm
    prompt: |
      {{YOUR_PROMPT_TEMPLATE}}
      
      {% for item in processedData %}
      {{item.text}}
      {% endfor %}
      
  # Use set-nodes instead of add-nodes when modifying the graph
  update-graph:
    input:
      data: generate-analysis
    tool: set-nodes
    nodes:
      - type: "{{NODE_TYPE}}"
        properties:
          title: "{{NODE_TITLE}}"
          content: "{{data.text}}"

functions:
  # Define reusable functions here
  custom-function:
    input:
      parameter1:
        type: text
      parameter2:
        type: tag
    actions:
      # Function implementation
      function-action:
        input:
          param1: function/parameter1
        tool: echo
        message: "{{YOUR_MESSAGE}}"
```

Replace all placeholders enclosed in `{{}}` with your specific values. Reference existing YAML files in the codebase (like `Theme_Document.yml` or files in the `Base Automations` directory) for examples of working implementations. 