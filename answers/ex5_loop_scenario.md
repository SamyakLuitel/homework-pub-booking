# Ex5 — Edinburgh research loop scenario

## Your answer

From the recorded session, the loop half first planned 3 subgoals
(sg_1..sg_3), then executed tools with a recovery loop before completion.
The first successful tool call was `venue_search` (summary says
"venue_search(Haymarket, party=6): 1 result(s)"). A second `venue_search`
attempt was rejected ("STOP calling venue_search..."). The next
`generate_flyer` attempt also failed because weather and cost were still
missing.

After that, the executor called `get_weather` and `calculate_cost`
successfully, then retried `generate_flyer` and wrote
`workspace/flyer.html` successfully. Finally it called `complete_task`
with `flyer_path: workspace/flyer.html` and marked the session complete.

So this run demonstrates the loop behavior clearly: attempt, validator
feedback, corrective tool calls, retry, then completion.

## Citations

- session/examples/ex5-edinburgh-research/sess_6f564752655d/SESSION.md
- session/examples/ex5-edinburgh-research/sess_6f564752655d/session.json
- session/examples/ex5-edinburgh-research/sess_6f564752655d/logs/trace.jsonl
- session/examples/ex5-edinburgh-research/sess_6f564752655d/workspace/flyer.html
