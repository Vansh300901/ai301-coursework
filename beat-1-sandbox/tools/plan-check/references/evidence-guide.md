# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

- Where it lives: The candidate plan's diagnosis section. Compare this against the repro evidence block in eval bundles, or the student's posted repro comment in live mode.
- What good looks like: The diagnosis cites specific behavior, trace lines, or artifacts from the repro evidence. It explains the root cause of the exact failure shown, without ignoring or contradicting any part of the evidence.

## Scope

- Where it lives: The candidate plan's scope statement.
- What good looks like: The scope explicitly states what will be changed and explicitly states what will *not* be changed. It draws a clear boundary around a single, focused fix, avoiding drive-by refactoring or unrelated changes.

## Executability

- Where it lives: The candidate plan's approach, order of work, and targeted files/areas.
- What good looks like: A stranger could take the plan, open the named files, and start implementing the logic described without needing to ask the author for clarification on what to do or where to do it.

## Test plan

- Where it lives: The candidate plan's test plan section, read against the steps in the repro evidence.
- What good looks like: The test plan describes concrete, observable actions (like re-running the exact repro steps) and states exactly what successful output or behavior will confirm the fix works. It proves the root cause is resolved, not just that the code compiles.

## Honesty

- Where it lives: The candidate plan's risks and unknowns section, and the deviations section (if applicable after a build).
- What good looks like: The author clearly acknowledges what they don't know, naming specific areas of uncertainty or potential side effects, rather than dressing up guesses as certainty. Deviations (if present) honestly explain why the original plan changed.

## Comms

- Where it lives: The candidate plan comment. Compare this against the issue's thread highlights (in an eval bundle) or live thread, and the repo-facts block.
- What good looks like: The comment addresses explicit constraints or questions raised by maintainers in the thread. It complies with the repo's stated conventions and contributing policies (like AI-use disclosure) rather than being generic, oblivious boilerplate.
