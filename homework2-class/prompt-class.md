# Codex Prompt: Dana's Bakery LangGraph Order System

## Task

Create a self-contained Google Colab Jupyter notebook named `dana_bakery_langgraph-class.ipynb` in this repository. Build the deterministic bakery-order workflow below with LangGraph. Do not call an LLM or require an API key. Include `%pip install` setup, a rendered graph diagram using `draw_mermaid_png()`, and executable test orders that cover every graph path and business rule.

## Interface

Provide an invocation compatible with:

```python
result = graph.invoke({
    "item": "babka",
    "quantity": 2,
    "day": "Thursday",
    "city": "Jerusalem",
})
```

The final result must contain these public fields: `status` (`accepted` or `rejected`), `item` (actual product sold, or `None`), `delivery` (`pickup` or `delivery`), `total` (amount due including delivery), `message` (warm deterministic reply), and `steps` (ordered diary entries).

## Catalog and inventory

| Product | Unit price | Stock |
| --- | ---: | ---: |
| challah | 18 | 10 |
| babka | 45 | 0 |
| rugelach | 30 | 3 |
| sourdough | 25 | 4 |

Only these products are sold; croissants are discontinued. Normalize item and city text by trimming whitespace and ignoring case. Reject unknown items, non-positive or non-integer quantities, and malformed weekdays politely. Valid days are Monday through Sunday.

## Business rules

1. Fulfill the requested quantity only when the full quantity is available.
2. If unavailable or insufficient, try successors in this exact chain: babka → rugelach → sourdough → challah. Preserve quantity and charge the substitute's price. Try at most two successors per order; every tested successor counts, even when unavailable. Accept the first successor with sufficient stock. If two suggestions fail or the chain ends sooner, apologize and reject.
3. Empty city means pickup. Jerusalem (case-insensitive) delivery costs 20. Other nonempty cities convert to free pickup and must be disclosed.
4. Avi's rule overrides Dana's: apply 15% off the baked-goods subtotal only when subtotal is strictly greater than 150 and the day is not Friday. Add delivery after discount. Round the total to two decimal places.
5. Replies use deterministic Python templates. No LLM, external service, Instagram, API key, or credentials.
6. Use typed LangGraph state and clear nodes for validation, stock checking, substitutions, fulfillment/delivery, pricing, reply creation, and rejection/cancellation. Use explicit conditional edges. Record every node/decision in `steps` in execution order, including rejected paths.
7. Render the compiled graph using `graph.get_graph().draw_mermaid_png()` and display it.

## Notebook organization

Use clear Markdown sections for setup, policy/state, graph construction, visualization, and tests. Keep cells focused and runnable top-to-bottom in a fresh Colab runtime. Install LangGraph without credentials. Explain that fixed morning inventory is not reserved or decremented between independent invocations.

## Tests and acceptance

Include assertions for invalid product and quantity; in-stock pickup; Jerusalem delivery; non-Jerusalem pickup conversion; each substitution transition (babka→rugelach, babka→sourdough after insufficient rugelach, rugelach→sourdough); cancellation after two failed suggestions and when the chain ends; discount boundary (150 gets none, above 150 on Thursday gets 15%); Friday exclusion; two-decimal total; all six public fields; and accurate ordered diary steps. Tests must not rely on state leaking between invocations.

Run all notebook cells from top to bottom in a fresh runtime, inspect outputs, and fix failures. Keep outputs free of secrets or personal data. Prepare the notebook for a public repository and accessible Colab link.

## Prompt revision note

Final prompt for the first implementation pass; no earlier rounds were used.
