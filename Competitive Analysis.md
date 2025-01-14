input:
  companyDocuments:
    type: document
    label: Company reports and documents

actions:
  openai:
    input:
      companyDocuments: task/companyDocuments
    tool: llm
    prompt: |
            Analyze the provided company documents and generate a comparative report focusing on common topics, consistency, and key findings.

            {% if companyDocuments %}
            Here are the company documents for analysis:
            {% for doc in companyDocuments %}
            ```Text
            {{ doc.xdoc.text }}
            ```
            {% endfor %}
            {% endif %}

            Based on the provided company documents, complete the following tasks:

            1. Topic Identification:
               a) Identify three common, significant topics discussed across the companies' reports.
               b) Briefly describe each topic and its relevance to the companies.

            2. Comparative Analysis:
               For each identified topic, conduct the following analysis:

               a) Consistency Analysis:
                  - Highlight incongruencies between companies' market perspectives
                  - Compare performance metrics within the topic category
                  - Analyze differences in presentation sentiment across companies

               b) Language Extraction:
                  - Extract specific language from the most recent quarters of all companies
                  - Include direct quotes that exemplify the analysis in step 2a
                  - Ensure quotes are properly attributed and contextualized

               c) Investor/Analyst Questions:
                  - Extract any investor or analyst questions related to this topic
                  - Categorize questions by company and provide brief context

               d) Related Documents:
                  - List related documents or sections for each topic
                  - Categorize by type (e.g., Analyst Summaries, Internal Documents)
                  - Tag with relevant metadata (e.g., year, quarter, document type)

            3. Overall Summary:
               a) Synthesize key findings across all topics and companies
               b) Highlight major trends, discrepancies, or notable observations
               c) Provide insights on potential implications for investors or analysts

            4. Output Format:
               Present your analysis in the following structure:

               COMPARATIVE REPORT

               1. [Topic 1 Name]
                  a) Consistency Analysis:
                     [Detailed analysis as per step 2a]
                  b) Extracted Language:
                     [Relevant quotes and language as per step 2b]
                  c) Investor/Analyst Questions:
                     [List of questions as per step 2c]
                  d) Related Documents:
                     [Categorized list as per step 2d]

               2. [Topic 2 Name]
                  [Repeat structure from Topic 1]

               3. [Topic 3 Name]
                  [Repeat structure from Topic 1]

               OVERALL SUMMARY
               [Synthesized findings and insights as per step 3]

            Ensure your response is comprehensive, objective, and based solely on the information provided in the company documents. Use clear headings and subheadings to organize the information effectively.