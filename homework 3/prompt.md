# Codex Implementation Prompt: LangGraph Support Ticket Router

## Provenance and status

This file is an implementation specification derived from the assignment PDF, the implementation plan approved in this conversation, and the user clarifications that followed. It was drafted alongside the notebook; it is not a verbatim transcript of an instruction supplied before the notebook was built.

## Technical specification

Create a small Google Colab notebook named `support_ticket_router.ipynb` that implements the requirements below. Keep the graph deliberately limited to the specified nodes and edges.

### Model and credentials
- Install `langgraph` and `langchain-openai` in the notebook with `!pip -q install langgraph langchain-openai`.
- Use `ChatOpenAI` with model ID `gpt-6-luna` and `temperature=0`. Keep the model ID in one `MODEL_NAME` constant. GPT-6 Luna is selected because it is an efficient low-cost model appropriate for choosing among three support categories; current official pricing lists it at $0.10 per million input tokens and $0.50 per million output tokens for standard short-context usage.
- Load the key only from Colab Secrets: `from google.colab import userdata` then `os.environ["OPENAI_API_KEY"] = userdata.get("OPENAI_API_KEY")`. Never hard-code, request, print, or expose the key in notebook output.

### State and classification
- Define a `TypedDict` state with exactly these required fields: `ticket`, `category`, `team`, and `reply`.
- Define a Pydantic structured-output model whose `category` is `Literal["billing", "technical", "general"]`.
- The `classify` node is the only LLM node and makes exactly one model call per ticket using `ChatOpenAI(...).with_structured_output(...)`. It reads the ticket and returns only `{"category": ...}`.
- Give the model concise category definitions: billing covers charges, payments, invoices, refunds, and subscriptions; technical covers bugs, crashes, error messages, and login problems; general covers feature ideas, general questions, and thanks. Tell it to classify by the ticket's underlying issue and meaning rather than a single keyword. It must decide mixed or ambiguous tickets semantically; do not add deterministic keyword routing or a fallback classifier.

### Graph
- Build exactly these nodes: `classify`, `billing_team`, `tech_team`, and `general_team`.
- Add one edge from `START` to `classify`.
- Add exactly one conditional edge from `classify`. Its routing function only reads `state["category"]`, and its mapping sends the three category values to their matching team nodes.
- Each team node is plain Python, makes no model call, and sets `team` and a fixed short `reply`. Use team values `Billing`, `Tech`, and `General`; replies must simply tell the customer the ticket was forwarded to that team.
- Add an edge from each team node to `END`. Do not add loops, tools, memory, extra nodes, or extra conditional edges.
- Compile the graph, render it with `draw_mermaid_png()`, and show the drawing in the notebook.

### Demonstration and tests
- Include a direct invocation in the requested form: `result = graph.invoke({"ticket": "I was charged twice this month"})`.
- Run at least seven distinct test tickets: at least two clearly billing, two clearly technical, two clearly general, plus the ambiguous example `The app crashes every time I try to pay`.
- Print ticket, category, team, and reply for every result. Assert all four result keys are present and validate that the six clear cases are classified correctly and collectively exercise all three branches. Report the ambiguous model decision without hard-coding its expected category.
- Keep the ticket examples short and do not print secrets.

## Revision tracking

This specification reflects the assignment, the approved implementation plan, and the later user clarifications. It documents the implementation requirements; it is not an exact transcript of the prompts exchanged in the conversation. Record any future changes to the implementation requirements here.
