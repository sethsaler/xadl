                                                                                
input:                                                                                                                                
  human:
    type: text
  context:                                                                                                                  
    type: assistant-context                                                                                                      
actions:
  respond:
    input:
      human: task/human
      context: task/context
    tool: llm                                                                                                                            
    prompt:                                                                                                                       
      - I am currently working on a document and need some help with analysis of the wording and subject matter in the document.
      - |                                                                                                                       
          {% if context.entity-by-type.xdoc %}                                                                        
          Here is the content of the document that I am working on:                                                             
          {% for item in context.entity-by-type.xdoc %}                                                               
          {{ item.text }}                                                                                                       
          {% endfor %}                                                                                                          
          {% endif %}                                                                                                           
      - |                                                                                                                       
          {% if context.focus %}                                                                                      
          When considering you response, please pay specific attention to the follow pieces of text:                            
          {% for item in context.focus %}                                                                             
          - {{ item.text }}                                                                                                     
          {% endfor %}                                                                                                          
          {% endif %}                                                                                                           
      - "Now that you understand the context, can you help me with this query: {{ human.text }}"