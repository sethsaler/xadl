<knowledge-set-instructions>
  <introduction>
    Use <project-plan>Project Plan.md</project-plan> and <file>File.yml</file> together to create the initial structural setup of a YAML file.
  </introduction>

  <steps>
    <step id="1">
      Create the YAML such that there is a document and workspace input.
    </step>
    <step id="2">
      Retrieve and utilize tags from the category <tag-category>[Tag Category or Categories]</tag-category>.
    </step>
    <step id="3">
      Initialize the knowledge set.
    </step>
    <step id="4">
      Set up appropriate dimensions/nodes.
    </step>
    <step id="5">
      Build out functions to iterate over each tag in <tag-category>[Tag Category or Categories]</tag-category> and evaluate content snippets for <task-instructions>[instructions for the task to be accomplished]</task-instructions>.
    </step>
  </steps>

  <additional-notes>
    <note>Remember that even singular tags will need to be evaluated as part of a for loop with a repeat function and a tag variable.</note>
    <note>Provide some well-reasoned structural output in the LLM tool section.</note>
  </additional-notes>
</knowledge-set-instructions>