# AGENTS.md

Short sections, in this order, each only if it has something true to say.
Aim for under 250 lines; move a long topic to `docs/` and link it.

1. **What it is**, in three sentences: the product, for whom, the languages
   (interface; repository). Then the stack in one line and the top-level
   folders with one clause each.
2. **Commands**: install (the one package manager, never the others), the
   fast check to run after every change, the full gate CI runs (`verify`),
   one test file, fix formatting. Generated files that are tracked and must
   never be edited by hand. How to add a dependency (`<pm> add <pkg>@latest`,
   never by editing the manifest).
3. **Local instance**: the `dev:agent` command, what it starts, what the
   banner prints, its flags, and what it refuses. Point to the test skill for
   checking a change in a browser.
4. **Ways to hurt yourself**: the hard limits from the audit, each with its
   reason and the safe alternative. Typical: killing processes by pattern on
   a shared host; touching containers, volumes or data you did not create;
   writing to production tools; printing or committing secrets.
5. **Domain**: the invariants a change must keep and where the code that
   enforces them lives. Not a tour of the code.
6. **Interface**: the owner's standing preferences (vocabulary, what not to
   add), only those broken more than once.
7. **Testing**: what deserves a test and what does not; a user-visible change
   is verified with the `browser-verify` skill.
8. **Documentation**: where decisions go (ADRs, never rewritten), what is
   frozen, and "never commit plans or scratch notes".
9. **Git and delivery**: from the user's answer, only the rules every session
   needs: never commit on `main`, a draft pull request after the first
   commit, commit subjects (conventional, no trailers), what must be green
   before ready, and exactly who merges and how. Writing the pull request,
   its captures and its review loop belong in the `pull-request` skill
   ([pull-request-skill.md](pull-request-skill.md)); point to it in one line.
