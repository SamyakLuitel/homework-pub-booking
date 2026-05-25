# Ex9 — Reflection

## Q1 — Planner handoff decision

### Your answer

In my Ex7 run (session sess_a382a2149fc1), the planner's second
subgoal was sg_2 "commit the booking under policy rules" with
assigned_half: "structured". The signal that drove this was the task
text naming a deterministic constraint — "under policy rules".
Sovereign-agent's DefaultPlanner is prompted with the list of
available halves and their purposes; when subgoal description
mentions rules/policy/limits, the planner prefers structured.

This decision is advisory, not physical. The orchestrator respects
it only because both halves are wired up. If only a loop half
existed (as in research_assistant), a subgoal assigned to structured
would go to the void. That's failure mode #4 from the course slides.

The broader lesson: the planner makes an architectural decision
based on prose interpretation. Put the rules somewhere the LLM
cannot mis-assign — in the structured half's Python — and prose
ambiguity no longer matters.

### Citation

- sessions/sess_a382a2149fc1/logs/tickets/tk_*/raw_output.json

The ex7 session shows the planner recommending a structured-hand off
but the orchestrator prevented an immediate handoff until the loop
half produced required artifacts (weather/cost/flyer). In
`sess_4fcfbc27a011` the loop called `venue_search` and attempted a
`handoff_to_structured`, and the handoff summary explicitly says
"Do not handoff yet. Call get_weather, calculate_cost, and
generate_flyer first." This demonstrates that planner assignments
can be advisory: the structured half is preferred when the task
requires deterministic policy checks, but the loop must still
produce the verifiable side-effects before structured confirmation.

Citation: [session/examples/ex7-handoff-bridge/sess_4fcfbc27a011/SESSION.md](session/examples/ex7-handoff-bridge/sess_4fcfbc27a011/SESSION.md#L1)


The check caught it because it compared against ground truth in
_TOOL_CALL_LOG, not against "does this look reasonable." The lesson
(Decision 3) I could rebuild as .jsonl files inside the session.

The ex5 session explicitly encodes a required tool sequence
(venue_search → get_weather → calculate_cost → generate_flyer →
complete_task). That rigid sequence means integrity checks should
validate both the trace events and the produced artifacts (for
example, `workspace/flyer.html`) against the tool-call outputs.
Ensuring the flyer content matches `calculate_cost` and other tool
results prevents subtle LLM hallucinations from slipping past a
human skim.

Citation: [session/examples/ex5-edinburgh-research/sess_6f564752655d/SESSION.md](session/examples/ex5-edinburgh-research/sess_6f564752655d/SESSION.md#L1)
cross-tenant data leaks, reconstructing per-run state from logs,
"how did this session end up this way" becomes SQL archaeology
instead of cat. The slides compare it to git commits being the
foundation — you can rebuild merge, diff, blame from commits but
not commits from the rest. Session directories are commits.


The available sessions reinforce that session directories and
cross-half coordination are the primitives you cannot easily remove.
- `sess_6f564752655d` (ex5) encodes required tool ordering and
	artifact outputs — losing session directories would make proving
	those side-effects harder.
- `sess_4be0aa5a001d` (ex6) shows the role of the structured half in
	deterministic confirmation: voice/loop should collect exact
	confirmation inputs for structured to consume.
- `sess_275799b09821` (ex8) highlights that ephemeral transports
	(voice vs text) matter only if the session artifacts and traces are
	preserved for grading.

Conclusion: keep session directories and the explicit trace / artifact
contracts; other primitives (ticket formats, IPC mechanisms) can be
replaced so long as the session-level guarantees remain.

Citation: [session/examples/ex5-edinburgh-research/sess_6f564752655d/SESSION.md](session/examples/ex5-edinburgh-research/sess_6f564752655d/SESSION.md#L1), [session/examples/ex6-rasa-half/sess_4be0aa5a001d/SESSION.md](session/examples/ex6-rasa-half/sess_4be0aa5a001d/SESSION.md#L1), [session/homework/ex8/sess_275799b09821/SESSION.md](session/homework/ex8/sess_275799b09821/SESSION.md#L1)
