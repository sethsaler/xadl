# Knowledge Set Instructions

Use **[Project Plan.md]** and **[File.yml]** together to create the initial structural setup of a YAML file. 

## Requirements

1. Create the YAML such that there is a document and workspace input.
2. The actions will need to get and use tags from a category called **[Tag Category or Categories]**.
3. Create the appropriate actions for:
   - Initializing the knowledge set
   - Setting up appropriate dimensions/nodes
   - Building out the functions that would iterate/loop over each of the tags within **[Tag Category or Categories]** to evaluate the content of the snippets with those tags for items **[instructions for the task to be accomplished]**.

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
    prompt: analysisPrompt
    title: analysisTitle
```

2. Key points about the structure:
   - Always use `repeat` with `function` and `for` parameters
   - The function name must be `analyze-for-tag`
   - The `for` parameter must be `[tag]`
   - Never use direct function calls with `function: analyze-for-category`
   - Keep all input parameters under the `input` section

## Additional Notes

- Remember that even singular tags will need to be evaluated as part of a for loop with a repeat function and a tag variable.
- The repeat structure is mandatory for all analysis actions, even when processing a single tag category.
- Provide some well-reasoned structural output in the LLM tool section.

## Common Issues to Avoid

1. ❌ Incorrect structure (Do not use):
```yaml
analyze-[category]:
  input:
    tag: get-[category]-tags
    documents: task/documents
  function: analyze-for-category
```

2. ✅ Correct structure (Always use):
```yaml
analyze-[category]:
  repeat:
    function: analyze-for-tag
    for: [tag]
  input:
    tag: get-[category]-tags
    documents: task/documents