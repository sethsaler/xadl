# Knowledge Set YAML Syntax Rules

## Basic Structure

A Knowledge Set YAML file consists of three main sections:

1. Input Declarations
2. Actions
3. Functions

## Input Declarations

```yaml
input:
  documents:
    type: document
  workspace:
    type: workspace
```

All input declarations must specify their type. Common types include:
- `document`
- `workspace`
- `text`
- `tag`
- `knowledge-set`

## Actions Section

Actions define the workflow of the knowledge set. Each action should follow this pattern:

### 1. Tag Collections
```yaml
get-[category]-tags:
  tool: find-tags
  [identifier]: [value]  # Either name or category
```

### 2. Prompt Configurations
```yaml
basePrompt:
  tool: echo
  message: |
    # [Context Title]
    [Role and context description]
    
    [Numbered objectives]
    
    [Analysis guidelines]
```

### 3. Knowledge Set Initialization
```yaml
init-kset:
  input:
    documents: task/documents
    [tagCollection1]: [get-tags-action1]
    [tagCollection2]: [get-tags-action2]
  tool: knowledge-set
  knowledgeSetName: "[Name]"
  operations:
    - op: add-dimension
      key: [dimension-key]
      name: [dimension-name]
      nodes: [tagCollection]
      priority: [number]
    - op: set-nodes
      nodes: [node-collection]
```

### 4. Analysis Actions
```yaml
analyze-[category]:
  repeat:
    function: analyze-for-tag
    for: [tag]
  input:
    tag: [tag-collection]
    documents: task/documents
    knowledge: init-kset
    basePrompt: basePrompt
    prompt: [analysis-prompt]
    title: [analysis-title]
```

## Functions Section

Functions define reusable components. Each function should follow this structure:

```yaml
[function-name]:
  input:
    [parameter]:
      type: [parameter-type]
  actions:
    get-snippets:
      input:
        tag: function/tag
        documents: function/documents
      tool: document-text
      textFilter:
        tags: [tag]
      skipIfEmpty: documents

    get-instruction-prompt-for-tag:
      input:
        tag: function/tag
      tool: echo
      message: |
        {% if [condition] %}
        [Specific format requirements]
        {% endif %}

    analyze-content:
      input:
        snippets: get-snippets
        basePrompt: function/basePrompt
        instructionPrompt: get-instruction-prompt-for-tag
        prompt: function/prompt
      tool: llm
      skipIfEmpty: snippets
      prompt: |
        {{instructionPrompt.text}}
        {{prompt.text}}
        {% for s in snippets %}
        {{s.text}}
        {% endfor %}

    create-insight:
      input:
        summary: analyze-content
        snippets: get-snippets
        tag: function/tag
        title: function/title
      skipIfEmpty: summary
      tool: insight
      text: |
        {{summary.text}}
      name: "{{title.text}} for {{tag.name}}"
      tags: [tag]
      attributes:
        category: [insight-category]
```

## Key Conventions

1. **Type Declarations**
   - All input parameters must declare their type
   - Common types: `document`, `workspace`, `text`, `tag`, `knowledge-set`

2. **Tool Usage**
   - `find-tags`: For tag collection
   - `echo`: For text templates and prompts
   - `document-text`: For text extraction with filters
   - `llm`: For content analysis
   - `insight`: For creating knowledge insights
   - `knowledge-set`: For managing knowledge sets

3. **Template Logic**
   - Use Liquid template syntax for conditional logic: `{% if condition %}`
   - Use dot notation for variable access: `{{variable.property}}`
   - Use loops for iterating over collections: `{% for item in collection %}`

4. **Naming Conventions**
   - Use kebab-case for dimension keys
   - Use PascalCase for knowledge set names
   - Use snake_case for function names
   - Use descriptive prefixes for actions (e.g., `get-`, `analyze-`, `create-`)

5. **Skip Conditions**
   - Use `skipIfEmpty` to prevent processing empty inputs
   - Specify the input that should be checked

6. **Priority Ordering**
   - Use numeric priorities in dimension definitions
   - Lower numbers indicate higher priority
   - Start from 1 and increment sequentially
