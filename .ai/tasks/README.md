# Task checkpoints

One file per in-flight task, so a conversation can be ended and replaced instead of grown.

1. Starting something that spans more than one sitting → copy `TEMPLATE.md` to
   `<short-kebab-name>.md`.
2. Working → keep **Completed**, **Current issue** and **Next steps** current.
3. Task done → delete the file. Durable outcomes belong in `docs/adr/`, `.ai/repo-map.md`,
   or the code.

State, not narrative. Under ~60 lines. Resume a new conversation with:
*"Read `.ai/tasks/<name>.md` and continue from there."*
