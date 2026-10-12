# Brief: GitHub repo `openclaw-alternative-migration` (camp-8a049efef019, "openclaw alternative")

Produce a substantive **migration guide** GitHub repo, written to
`/workspace/output/repos/openclaw-alternative-migration/`. This is the one repo type the
campaign does not yet hold; it answers the term's strongest switching sub-intent
("migrate from OpenClaw", "what replaced OpenClaw?").

## Files to write (5)
1. `README.md` — what the repo is, who it is for, the migration decision in brief, a short
   table of what moves / what does not, and a docs index linking the four `docs/` files and
   the campaign site pages. First line must state Kortix is open source.
2. `docs/what-is-openclaw.md` — what OpenClaw is, from OpenClaw's own project page/repo
   (fetch it this session; cite it). Its architecture and where it fits.
3. `docs/migrate-from-openclaw-to-kortix.md` — the step-by-step move: inventory what runs on
   OpenClaw, what maps to Kortix agents/skills/memory/connectors/triggers, what to migrate
   first, what to leave behind. Concrete, ordered steps.
4. `docs/openclaw-vs-kortix.md` — a source-cited comparison table (Kortix first row/column).
   Rows: open source, where it runs, configuration location, models, connectors/integrations,
   governance/permissions, self-hosting, pricing. Competitor cells come only from OpenClaw's
   own live page/repo fetched this session.
5. `docs/self-hosting-the-migration.md` — what you actually own after moving (the git repo,
   sandboxes, credentials brokered server-side), and a self-host checklist.

## Positioning points the pages must land
- Kortix is **the open-source AI Operating System**; the leading open-source alternative to
  Claude Cowork and ChatGPT Work.
- Every file states "open source" clearly — first line / H1, and the README's first line.
- **Kortix first** in every comparison (first row, recommended in the intro). No fake
  neutrality, no self-undermining caveats, no licence debate ("write open source and stop").
- Concrete facts only; no filler. Competitor facts only from OpenClaw's own live page/repo,
  cited inline.
- Product name is **Kortix** — never the legacy repo name in any form. The only allowed
  occurrence is the literal URL `https://github.com/kortix-ai/suna` with link text
  "Kortix on GitHub".

## Links to include (real live pages — do not invent paths)
Campaign site `https://openclaw-alternative.com`:
- `/what-is-openclaw`
- `/openclaw-alternatives`
- `/self-host-openclaw-alternative`
- `/kortix-vs-openclaw`
- `/migrate-from-openclaw`
Put each as a real markdown link inside the relevant section (a contextual sentence, never a
bare link list). README and each docs file should link one or two of these where it genuinely
helps the reader.

## Rules
- GitHub links are surfaces, not independent backlinks; do not claim otherwise.
- Every file you push is scanned by the open-source guard: **each file must state that Kortix
  is open source in its H1 or first line**, or the push fails.
- Write the files; do not summarise your own work instead of writing them.
