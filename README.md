# virajapalleti-langsmith-MAT496

## INSIGHTS:

## **Module01:**

_Lesson02: Simple Graphs_  
Built a basic LangGraph workflow demonstrating conditional branching - the foundation of decision-making in graphs.

- State (similar to a dictionary)= obj e pass between nodes and edges of a graph
- Learnt Graph construction and node definition
- Conditional edges for branching logic and then Graph visualisation

Made my own graph implementation with 5 nodes with probability fo 0.25 each for choosing my favourite f1 driver.  
[module01/SimpleGraph.ipynb](module01/SimpleGraph.ipynb)

_Lesson03: Langgraph studio_  
Requires no code, learnt how to navigate through Langgraph Studio and visualised the graph implemented in the previous notebook.  
Also made it for my own implemented graph!  
[module01/LanggraphStudio.ipynb](module01\LanggraphStudio.ipynb)

_Lesson04: Chain_  
Through this I've learnt how chat models interact with tools through graphs execution. Model request tool calls --> graph executes them --> results come back as msgs.

- MessageState = conversation history stored as a list of msgs using "add_messages" reducer.
- Tool binding and execution = attach func to chet models, run tools and then gets back the result to the msg stream.  
  [module01/Chain.ipynb](module01/Chain.ipynb)

_Lesson05: Router_  
Learnt that the graph routes ebtween LLM responses and the tool execution based on model output.  
(Created a tool and added it to the model's available tools list. The LLM sees the tool and decides on its own whether it needs it or not.)

- tools_condition checks if model requests a tool call
- Conditional edge routes to ToolNode if tool needed, or ends if direct response sufficient

This is way smarter than hardcoded if/else statements as the LLM dynamically decides the routing based on understanding the user's question, not pre-programmed rules.  
[module01/Router.ipynb](module01/Routuer.ipynb)

_Lesson06: Agent_  
Built an agent that can use multiple tools in sequence. The model doesn't just call one tool and stop - it keeps going in a loop until needed. It is not pre-programmed.

- Calls tool based on the circumstance
- Tool gives output
- If the output is not an 'end' outcome, this output is given back
- Process repeats until task is done  
  [module01/Agent.ipynb](module01/Agent.ipynb)

_Lesson07: Agent Memory_
Added memory to our agents. Created a thread_id to maintain conversation context. When asking follow-up questions that reference earlier ones, use the same thread ID. Now, the agent "remembers" the results and tools it got earlier.

- Same thread ID = agent has full history, can reference past interactions
- Different thread ID = fresh start, no memory of previous conversation  
  [module01/AgentMemory.ipynb](module01/AgentMemory.ipynb)
