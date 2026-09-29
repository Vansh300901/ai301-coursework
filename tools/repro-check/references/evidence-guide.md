# Evidence guide: where proof lives in a reproduction package

## Environment

- **Where it lives**: In eval mode, inspect the repro report's environment block or header, read against the `repo-facts` block and the original issue's reported runtime. In live mode, look at the system details section of the draft report.
- **What good looks like**: Explicitly names the operating system, runtime/language version (e.g., Python 3.11), and git commit hash or package release version. The versions either match the issue or note any variance.

## Steps

- **Where it lives**: In the reproduction steps section of the repro report.
- **What good looks like**: A sequential, self-contained list of commands or actions from starting state to error trigger that an independent person could execute in order without missing prerequisite files, packages, or commands.

## Behavior shown

- **Where it lives**: In the repro report's artifacts block (terminal outputs, stack traces, log excerpts, test runner outputs), read against the issue description and error details.
- **What good looks like**: The raw output directly demonstrates the specific failure, exception, or visual defect reported in the issue (e.g., the exact traceback or assertion failure), rather than an unrelated dependency import failure, setup error, or adjacent symptom.

## Honesty

- **Where it lives**: At the intersection of the report's conclusion and its raw artifact block.
- **What good looks like**: The written conclusion accurately states what happened. If the bug failed to trigger, it honestly reports "cannot reproduce" backed by terminal output showing the test passing or unexpected clean exit. It never claims a fix or successful reproduction that the logs do not support.

## Comms

- **Where it lives**: The claim comment, the repro report text, and the `repo-facts` contribution policy (specifically sections on AI guidelines, PR/issue templates, or disclosure walls).
- **What good looks like**: The claim comment references the specific issue and promises investigation rather than premature fixes. If the repository policy mandates AI-use disclosure, the comment or report explicitly includes the required disclosure statement.
