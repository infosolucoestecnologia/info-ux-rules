# info-ux-rules

A Claude skill with the UX/UI rules used across Info Soluções systems (JS, Info Cardápio, Info Ponto, Sistema Info, Dashboard V2).

Once installed, Claude applies these rules whenever you ask it to review, critique, redesign, or build a screen, dashboard, form, list, table, or component. The rules cover upload, buttons and forms, tables on mobile, modal vs sheet, hierarchy, search and filter, bulk actions, long forms, hover on touch, typography, and required light and dark themes. The full rule set lives in [`SKILL.md`](SKILL.md).

## Install

Clone this repository into your Claude skills folder:

```bash
git clone https://github.com/infosolucoestecnologia/info-ux-rules.git ~/.claude/skills/info-ux-rules
```

Or copy the folder manually so that `SKILL.md` ends up at `~/.claude/skills/info-ux-rules/SKILL.md`.

Restart Claude Code (or start a new session) and the skill will be picked up automatically.

## Usage

Just ask Claude to review or build a screen, for example: "review this screen", "does this look right on mobile", "improve this layout", or "revisa essa tela". Claude will read the core principles, then only the sections that match what the screen contains, and report problems worst first.
