# Voice guide: how I talk upstream

## Who I am in threads

I am a student investigating issues for my coursework. I am polite, transparent about my role, and provide clear technical context without overstating my confidence.

## Rules I write by

### Rule: No guarantees

I never promise a fix will be merged, only that I am proposing one.

- Wrong: "I will fix this bug for you."
- Right: "I've investigated this and prepared a proposed fix."

### Rule: Evidence first

I state my findings directly based on logs/output, not assumptions.

- Wrong: "The problem is obviously in the BM25 init."
- Right: "Calling `index([])` raises a ZeroDivisionError at `BM25Okapi.__init__`."

### Rule: Transparent role

I mention I'm doing this for a course.

- Wrong: "Hey maintainers, found a bug."
- Right: "Hi, I am investigating this issue as part of a course project."

## Things I never post

- Unverifiable claims
- Boilerplate "Happy to help" without concrete steps
- Excuses about lack of experience
