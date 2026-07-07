This directory contains a real, already-built, working shared CSS design
system (`tokens.css`, `base.css`, `provenance-panel.css/.html`, `README.md`)
used by a small family of related websites (purplepincher.org, deckboss.net,
cocapn.ai, and more to come). It's been verified syntactically valid and is
already deployed live in at least one site (deckboss.net). The org's own
decision (from a design-review process) is: publish this as a small, real,
public repo now — quietly, no fanfare, just a plain description, consistent
with how the org treats everything else it ships. Not "look how organized we
are," just "this is the actual CSS the family's sites are built from."

## Your job

Prepare this directory to actually become a clean, minimal, publishable
package/repo. Specifically:

1. Add a `package.json` — name it something plain and accurate (e.g.
   `@purplepincher/family-styles` or similar — your call, keep it boring and
   accurate), version 0.1.0, a one-line description matching the "quiet,
   not a launch" tone, MIT license (matching the org's convention elsewhere).
2. Lightly revise `README.md` if needed so its tone matches "here's the CSS
   the family's sites use" rather than anything that reads as an
   announcement or a pitch — check it against the existing file; if it
   already reads right, leave it alone and say so rather than rewriting for
   the sake of rewriting.
3. Add a `.gitignore` (minimal — node_modules if relevant, OS cruft) and a
   `LICENSE` file (MIT, matching the org's other repos).
4. Add ONE small, real, working example: a tiny `example/demo.html` that
   actually imports/inlines `tokens.css` and `base.css` and renders a
   minimal page using a couple of the shared component classes (`.eyebrow`,
   `.chain`, `.ledger`) with placeholder content, so someone encountering
   this repo cold can see the system actually working in a browser, not just
   read about it. This should be openable directly in a browser with no
   build step.

Do not add a build tool, a bundler, a test framework, or any other
infrastructure beyond what's listed above — this is deliberately minimal
packaging for a small CSS file set, not a software product. Keep it that
plain, on purpose.
