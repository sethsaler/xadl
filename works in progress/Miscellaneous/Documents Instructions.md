# Instructions for Recreating a YAML with Different Objectives

1. **Retain the same file structure:**
   - Use the same root key (e.g., `draft_answers`).
   - Under that key, create a list (or array) of items.
   - Each item in the list is a mapping (dictionary) containing at least two keys. By default, these are:
     - `objective`
     - `answer`

2. **Ensure proper indentation and formatting:**
   - YAML relies on consistent indentation.
   - Each item should begin with a hyphen (-) followed by a space.
   - Keys and values are separated by a colon (:) and a space.
   - When adding text beyond a single line, you can either:
     - Keep it on the same line if it's short.
     - Use the YAML multiline format (e.g., a pipe `|` or a `>` symbol) if the text is longer or you want to preserve formatting.

3. **Use unique content for each objective and answer:**
   - Replace the original objectives with new ones relevant to your use case.
   - Provide answers or descriptions that fit each new objective.

4. **Include additional keys or information if needed, but maintain the original keys:**
   - You may add more keys (for instance, `details`, `deadline`, or `notes`) under each item if you want to store more data.
   - However, do not remove or rename the original keys (`objective` and `answer`) unless your use case specifically requires it.
   - If you do add new keys, maintain the same indentation level as `objective` and `answer`.

5. **Validate the generated YAML structure before finalizing:**
   - Confirm that all indentation levels are correct.
   - Confirm that each list item is prefixed with a hyphen.
   - Confirm that all mandatory keys (`objective` and `answer`) are present.

6. **Provide an example of the final format:**
   - Once you've produced the new YAML, you can optionally provide a short example.

7. **When prompting the LLM, clearly specify the new objectives and any extra keys:**
   - Provide the LLM with a list of objectives you want to include.
   - Provide any additional text or details you want inserted into `answer`, `notes`, `details`, etc.
   - Emphasize maintaining the YAML structure (indentation, hyphens, etc.).

8. **Final output verification:**
   - After the YAML is generated, verify that it doesn't have syntax errors (you can optionally run it through a YAML validator).
   - Confirm that all keys and objectives match what you requested from the LLM.