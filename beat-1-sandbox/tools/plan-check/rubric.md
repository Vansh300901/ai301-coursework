# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diagnosis evidence match | The plan's stated diagnosis read against the repro evidence's trace. | The diagnosis directly explains the specific error or behavior shown in the repro evidence without ignoring conflicting details. | required |
| Scope bound | The plan's scope statement. | The change is bounded, naming specifically what will be changed and explicitly acknowledging what will not be touched. | required |
| Root cause vs symptom | The plan's approach read against the repro evidence. | The approach targets the root cause of the behavior rather than merely suppressing the symptom. | required |
| Executability | The plan's approach. | A stranger could execute the plan without needing to ask for clarification on the steps or missing details. | required |
| Test plan observable | The plan's test plan read against the repro evidence's steps. | The test plan executes steps that produce observable evidence confirming the fix, mirroring or adapting the repro steps. | required |
| Unknowns acknowledged | The plan's risks and unknowns section. | The plan acknowledges its uncertainties rather than dressing up unknowns as certainty. | required |
| Thread and convention compliance | The plan comment read against the thread highlights and repo-facts block. | The plan comment addresses any explicit constraints or requests from the issue thread and adheres to repo conventions. | required |

## Verdict rule

Accept (ready) if every required check passes. Unclear counts as fail. Reject (hold) if any required check fails.
