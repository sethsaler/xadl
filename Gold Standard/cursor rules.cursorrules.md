Below is an example of how you can document this modified YAML syntax in a Markdown file. It explains the core ideas and provides code snippets to illustrate each lesson. You can copy/paste this into your own `.md` file and adjust as needed.

---

# Custom YAML Syntax for LLM-Based Pipelines

This document describes a custom YAML syntax for defining inputs, actions, and functions in a Large Language Model (LLM) workflow. The syntax allows you to:

1. Declare input sections (the data used by the workflow)  
2. Define actions (operations performed on that data)  
3. Define reusable functions (collections of sub-operations)  
4. Use “repeat” constructs (iterate over multiple inputs)  
5. Conditionally control logic flow  
6. Loop over outputs in your prompts  

By understanding and combining these elements, you can build highly flexible LLM-driven processes.

---

## Lesson 1: Defining Input Sections

In this syntax, the `input` section specifies the types of data the workflow will use. For example:

```yaml
input:
  documents:
    type: document
  workspace:
    type: workspace
```

- **documents**: Declares a data source of type `document`.  
- **workspace**: Declares a data source of type `workspace`.

These declarations make the data accessible to the subsequent actions and functions.

---

## Lesson 2: Defining Actions

Actions specify operations to be performed on the inputs. For instance:

```yaml
actions:
  get-agreementInformation-tags:
    tool: find-tags
    name: ["Credit", "Preamble"]
```

- **get-agreementInformation-tags**: The action’s name.  
- **tool**: The tool or operation that this action uses—here it is `find-tags`.  
- **name**: The argument(s) passed to the tool—here, it looks for tags named `"Credit"` and `"Preamble"`.

Actions are declared under the `actions:` key, and each action can accept parameters or arguments as needed by the associated tool.

---

## Lesson 3: Defining Functions

Functions are reusable blocks of code that can be used in multiple actions. Each function has inputs and one or more actions. For example:

```yaml
functions:
  analyze-for-tag:
    input:
      tag:
        type: tag
      agreementTag:
        type: function/agreementTag
      documents:
        type: function/documents
      textFilter:
        operator: "and"
        tags: [tag, agreementTag]
    actions:
      get-snippets:
        input:
          documents: function/documents
        tool: document-text
        prompt: "Extract snippets based on the tags"
```

- **analyze-for-tag**: The function’s name.  
- **input**: Describes what data (and their types) this function needs:
  - `tag`, `agreementTag`, `documents`, etc.  
- **actions**: One or more actions inside the function, e.g., `get-snippets` calls the `document-text` tool.

By placing logic in functions, you can reuse them in different contexts, reducing duplication.

---

## Lesson 4: Using Repeat Functions

A repeat function allows you to call the same function multiple times over different inputs:

```yaml
analyze-each-documentCategory-tag:
  repeat:
    function: analyze-for-tag
    for: [tag, agreementTag]
  input:
    tag: get-documentCategory-tags
    agreementTag: get-agreementInformation-tags
    documents: task/documents
    knowledge: init-kset
    basePrompt: basePrompt
    prompt: contractAnalysisPrompt
    title: contractAnalysisTitle
```

- **analyze-each-documentCategory-tag**: The action name.  
- **repeat**: Specifies that this action will repeatedly invoke `analyze-for-tag`.  
- **for**: Lists which input parameters should be cycled over—for example, `[tag, agreementTag]`.  

In this snippet, each combination of `tag` and `agreementTag` is fed into the `analyze-for-tag` function.

---

## Lesson 5: Adding Results to the Workspace

You can add results to a shared `workspace` by using the `workspace` tool. For example:

```yaml
add-to-workspace:
  input:
    workspace: task/workspace
    documentCategoryAnalysis: analyze-each-documentCategory-tag
    knowledgeForDocuments: init-kset
  tool: workspace
  operations:
    - add-items
      items: knowledgeForDocuments
```

- **add-to-workspace**: The action name.  
- **workspace**: A reference to an existing workspace data source.  
- **operations**: Steps to be performed on the workspace. Here, `add-items` is used to insert `knowledgeForDocuments` into it.

---

## Lesson 6: Using Conditional Statements

Use conditional statements within actions or functions to control the flow. For instance:

```yaml
get-instruction-prompt-for-tag:
  input:
    agreementTag: function/agreementTag
  tool: echo
  message: |
    {% if agreementTag.name == "Preamble" %}
    # Define the instructions for the "Preamble" tag
    PREAMBLE instructions:
    ...
    {% endif %}
```

- **get-instruction-prompt-for-tag**: An action that checks if `agreementTag.name` equals `"Preamble"`.  
- When true, it outputs instructions specific to the “Preamble” tag.

You can use this pattern to selectively execute YAML sections based on runtime conditions.

---

## Lesson 7: Using Loops

Loops allow you to repeat an action for multiple items, for instance, each “snippet” in a list:

```yaml
run-prompt:
  input:
    snippets: get-snippets
    basePrompt: function/basePrompt
    instructionPrompt: get-instruction-prompt-for-tag
    prompt: function/prompt
  tool: llm
  prompt: |
    {% for s in snippets %}
    {{s.text}}
    {% endfor %}
```

- **snippets**: A list of text snippets to be iterated over.  
- **{% for s in snippets %}**: The loop; each snippet is rendered in the final prompt.  
- **llm**: The tool (in this case) that processes the prompt.

By combining loops with your LLM calls, you can process a series of items in a single action.

---

## Conclusion

These lessons illustrate how to build input sections, actions, functions, and control structures in your custom YAML syntax for LLM workflows:

1. **Define** your inputs (documents, workspace, etc.).  
2. **Implement** actions to perform specific tasks.  
3. **Create** reusable functions for complex processes.  
4. **Use** repeat constructs to iterate over arrays or multiple inputs.  
5. **Add** results to a shared workspace for persistence.  
6. **Apply** conditionals to dynamically control logic flow.  
7. **Leverage** loops in your prompts to process multiple items seamlessly.

Experiment with different scenarios to deepen your understanding of this approach. By structuring your LLM workflows in this manner, you can build scalable, maintainable, and extensible pipelines.