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
