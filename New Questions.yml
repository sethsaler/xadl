input:
  earningsCallDocuments:
    type: document
    label: Earnings call transcripts

actions:
  openai:
    input:
      earningsCallDocuments: task/earningsCallDocuments
    tool: llm
    prompt: |
        Analyze earnings call transcripts to identify analysts, create profiles, and generate potential future questions.

        {% if earningsCallDocuments %}
        Here are the earnings call transcripts for analysis:
        {% for doc in earningsCallDocuments %}
        ```Text
        {{ doc.xdoc.text }}
        ```
        {% endfor %}
        {% endif %}

        Complete the following tasks based on the provided earnings call transcripts:

        1. Analyst Identification:
           a) Search the documents for all analysts asking questions in the Q&A portions
           b) Create a list of identified analysts and their associated financial institutions

        2. Analyst Profiling:
           For each identified analyst, create a profile including:
           a) Name and financial institution
           b) Questioning style (e.g., direct, analytical, focused on specific areas)
           c) Particular interests or recurring themes in their questions
           d) Any unique characteristics or patterns observed

        3. Generate Potential Future Questions:
           For each analyst profile:
           a) Devise 5-10 non-obvious, complex questions they might ask in future calls
           b) Ensure questions are novel and not mere restatements of previously asked questions
           c) Base questions on the analyst's profile, interests, and questioning style
           d) Consider current market trends and company developments in formulating questions

        4. Question Justification:
           For each generated question:
           a) Provide a brief explanation of how it aligns with the analyst's profile
           b) Highlight how it differs from or builds upon previously asked questions

        Present your analysis as a structured report with clear sections for each analyst. Include the analyst profiles and the list of potential future questions with their justifications. Ensure that the generated questions are insightful, relevant, and aligned with each analyst's demonstrated interests and style.
