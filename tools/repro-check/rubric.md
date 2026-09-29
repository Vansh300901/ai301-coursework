# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `environment-recorded` | Repro report environment section read against repo-facts runtime requirements | Specifies the concrete environment details used for reproduction (operating system, Python/runtime version, and repository commit SHA or installed package version). | required |
| `followable-steps` | Repro report reproduction steps and commands | Provides clear, self-contained, reproducible commands or steps that allow an independent developer to execute and reach the same state without missing prerequisite setup steps. | required |
| `target-matched` | Repro report observed behavior and error output read against the original issue description | The observed outcome or error directly targets and matches the specific defect described in the issue, rather than an unrelated dependency error, missing setup failure, or adjacent symptom. | required |
| `evidence-shown` | Repro report actual output, stack trace, or logs | Includes direct, unedited command output, relevant stack trace, or log excerpt demonstrating the failure (or concrete evidence showing inability to reproduce despite running the steps). | required |
| `policy-disclosure-compliant` | Claim comment, repro report, and repo-facts contribution/AI policy | If the repository policy mandates disclosure of AI assistance or tool usage, the comment or report explicitly includes the required disclosure statement. | required |
| `honest-outcome` | Repro report conclusion and observed behavior | Explicitly and accurately states whether the bug was successfully reproduced or could not be reproduced based on the evidence presented, without overclaiming or asserting an unverified fix. | required |
| `clean-formatting` | Comment markdown formatting | Uses fenced code blocks for multi-line terminal outputs and stack traces. | preferred |

## Verdict rule

Accept if and only if every `required` check receives a pass (`P`). If any `required` check receives a fail (`F`) or unclear (`?`), the verdict is `reject`. Checks marked `preferred` never change the binary verdict.
