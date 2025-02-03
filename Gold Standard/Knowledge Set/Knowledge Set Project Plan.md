<project-plan>
  <objective>
    Create or modify a YAML workflow that finds specific tags (e.g., "Definition &amp; Scope of Confidentiality"), sets up prompts for LLM-based analysis, extracts relevant snippets, and saves summarized results to a knowledge set and workspace.
  </objective>
  <steps>
    <step id="1">
      Find tags under "Definition &amp; Scope of Confidentiality".
    </step>
    <step id="2">
      Define a base prompt for the lawyer persona and an analysis prompt for summarizing obligations.
    </step>
    <step id="3">
      Initialize a knowledge set referencing relevant documents and the tags.
    </step>
    <step id="4">
      Analyze each tag to retrieve relevant snippets and run them through an LLM.
    </step>
    <step id="5">
      Save the resulting summaries into an insight object and publish everything to the workspace.
    </step>
  </steps>
  <metadata>
    <note>
      Use tools as follows: [find-tags, echo, knowledge-set, document-text, llm, insight, workspace] with inputs in the format [function/inputName].
    </note>
  </metadata>
</project-plan>