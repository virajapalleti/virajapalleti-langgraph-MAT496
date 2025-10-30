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

## **Module02:**

_Lesson01: State Schema_
Schema defines what data structure your graph uses - basically the "shape" of the data flowing through your graph. Schema makes graphs predictable - you know exactly what's available at each step and how updates behave.

- TypedDict = Define state as a Python TypedDict specifying exact fields and types. Every node reads from and writes to these fields.
- Pydantic = While TypedDict just defines types, Pydantic actually enforces them at runtime and validates data.

(catches type errors immediately when state is updated)
[module02/StateSchema.ipynb](module02/StateSchema.ipynb)

_Lesson02: State Reducers_

- Reducers allow us to specify how to perform state updates.
- When nodes run in parallel and both update the same field; without a reducer: condition - one overwrites the other randomly
- We use: Annotated[type, reducer]  
  [module02/StateReducer.ipynb](module02/StateReducer.ipynb)

_Lesson03: Multiple Schemas_

- Useful for restricting what is present in the input and output schemas of a graph. ( Basically encapsulation)
- We have; Input Schema, Private Schema(the other two nodes communicate using this) and Output Schema.
  [MultipleSchemas.ipynb](module02/MultipleSchemas.ipynb)

_Lesson04: Trimming and Filtering Mesages_
Sending the entire history of a conversation to the LLM wastes tokens, hits length limits and is slower. Thus we trim and filter msgs:

- Trimming = keeps only the most recent 'x' msgs or msgs within the token budget
- Filtering = Removes specific msg types like tool calls etc. Role-based mssg can also be filtered.
  [Trim-FilterMessages.ipynb](module02/Trim-FilterMessages.ipynb)

_Lesson05: Chatbot w/ Summarizing Messages and Memory_

- After N messages (notebook uses 6), we trigger the summarization node
- Summary node summarises old messages into brief outline
- Replaces old messages with summary + keep recent messages (or deletes 2 msgs, depending on user preference)
- We also only use the summarise msgs if the len(messages) is over a particular threshold.
- LangGraph's checkpointer automatically saves state including summaries. When conversation resumes (of the same thread_id), summary is reused.
- Better/practical approach for token usage.  
  [ChatbotSummarization.ipynb](module02/ChatbotSummarization.ipynb)

_Lesson06: Chatbot w/ Summarizing Messages and External Memory_  
When a notebook restarts/server reboots, all the conversation history disappears as by default LangGraph stores state in RAM. Thus we used external checkpoints, to persist state to actual databases so the conversations remain even after restart.

- Uses SQLite database file to store every thread's complete state
- Combining with summarization techniques, we dont have to pay to re-summarize the summary that's already saved in the checkpoint, even after resuming.  
  [ChatbotExternalMemory.ipynb](module02/ChatbotExternalMemory.ipynb)

## **Module03:**

_Lesson01: Streaming_  
Streaming gives visibility into the graph's execution process, whether you want to see everything (values) or just the changes (updates)  
Value Mode:

- Displays the complete graph state after each node executes
- See the full picture at every step

Updates Mode:

- Only shows what changed when a node is called  
  [module03/StreamingInterruption.ipynb](module03/StreamingInterruption.ipynb)

_Lesson02: Breakpoints_  
Learned why breakpoints are needed (safety, control, quality) and three use cases: approval for authorizing actions, debugging, and editing for modifying execution.
Learnt to apply pause points during agent execution that allow human intervention.  
[Breakpoints.ipynb](module03/Breakpoints.ipynb)

_Lesson03: Editing state and Human feedback_  
Can directly modify graph state at breakpoints using update_state function and enables realtime correction of agent behavior during the execution.  
It's best useful if the agent is running in a wrogn direction because I can just jump in to correct.

- Asked model to do something -> Hits breakpoint and edited the command -> Model continued with updated instructions  
  [EditGraphState.ipynb](module03/EditStateHumanFeedbac.ipynb)

_Lesson04: Dynamic Breakpoints_  
Regular breakpoints fire every time regardless of input but dynamic ones only trigger when specific conditions are met. (We can aslo include custom error messages). These are evaluated inside nodes during execution.

- Use NodeInterrupt to check conditions within a node -> If condition is true, graph pauses itself -> Resume using update_state() like before  
  [DynamicBreakpoints.ipynb](module03/DynamicBreakpoints.ipynb)

_Lesson05: Time Travel_
(It's called time travel because you can go back into any point of executon and change/review it)  
Each node execution creates a checkpoint with a unique ID and Checkpoints store the complete state at that point.  
Replaying = Observe the flow from a past checkpoint without re-executing (Just view what happened from that point forward)  
Forking = Re-run from a checkpoint with different input (Make edits to the state and execute from that point onwards)

- get_state_history() - View full state history of the graph
- graph.stream(checkpoint_id=...) - Jump to any checkpoint to replay or fork  
  [TimeTravel.ipynb](module03/TimeTravel.ipynb)

## **Module04:**

_Lesson01: Parallelization_

- fanning out means sending one node’s output to multiple parallel nodes, while fanning in combines results from multiple nodes into one.
- reducer = updates all the merges safely.
- custom reducer can control the execution order of parallel states (for consistent updates).
- operator.add helps combine values correctly instead of overwriting them.  
  [Parallelization.ipynb](module04/parallelization.ipynb)

_Lesson02: Sub-Graphs_  
Understood the concept of subgraphs, which allow dividing a main graph into smaller, independent sections that manage their own states. They improve controllabity by enabling to operate separately and still being connected to the workflow.   
Parent graphs communicate with subgraphs through overlapping keys, enabling data sharing between them. Multi-agent systems = each agent can have its own subgraph to handle specific tasks.  
[SubGraph.ipynb](module04/SubGraph.ipynb)
