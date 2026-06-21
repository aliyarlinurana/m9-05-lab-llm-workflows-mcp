# Manual Tool-Use Loop — Transcript

## Two-turn memory demo

### Turn 1
**User:** What did order A1001 cost?

--- Step 1 ---
Model requested tool: lookup_order({'order_id': 'A1001'})
Tool result: {'order_id': 'A1001', 'item': 'laptop', 'price': 1200, 'purchased': '2026-05-20', 'warranty_months': 12}

--- Step 2 ---
Model gave final answer: Order A1001 cost $1,200.

### Turn 2
**User:** And what about three of them?

--- Step 1 ---
Model requested tool: calculate({'expression': '1200 * 3'})
Tool result: {'result': 3600}

--- Step 2 ---
Model gave final answer: Three of them would cost $3,600.

**Why this proves memory works:** Turn 2's question never mentions "A1001" or "1200" — the model only knew the price because it was still present in the `messages` list from Turn 1's tool result.

## Step limit demo

**User:** For order A1001, what would the total be if I bought three of them?
**max_steps = 1** (artificially low, to force the limit)

--- Step 1 ---
Model requested tool: lookup_order({'order_id': 'A1001'})
Tool result: {'order_id': 'A1001', 'item': 'laptop', 'price': 1200, 'purchased': '2026-05-20', 'warranty_months': 12}

Couldn't finish in time (hit step limit).

**Why this proves the limit works:** the task needs two tool calls (lookup then calculate), but the loop was capped at 1 step, so it stopped cleanly instead of looping forever or crashing.