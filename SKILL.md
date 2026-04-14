---
name: vsc-tricks
description: VS Code workspace customization utilities — color borders, settings scaffolding
argument-hint: colorize [color]
---

# VSC Tricks

**Current version**: 0.1.0

VS Code workspace customization utilities. Creates and manages local `.vscode/settings.json` with visual customizations for your project windows.

**This skill requires explicit invocation via `/vsc-tricks`.** It does not activate automatically based on conversation content.

---

## Actions

When `/vsc-tricks` is invoked without a subcommand, present this menu:

```
**Workspace Customization**
- `colorize` - Set random or specific window border/title bar colors for this project
```

Then ask: "Which action would you like to run?"

---

## Routing

| Invocation | Action File |
|------------|-------------|
| `/vsc-tricks colorize` | `actions/colorize.md` |
| `/vsc-tricks colorize blue` | `actions/colorize.md` (with color hint "blue") |
| `/vsc-tricks colorize #FF5733` | `actions/colorize.md` (with explicit hex) |
| `/vsc-tricks color` | `actions/colorize.md` |

**Argument passing:** Anything after `colorize` is passed as the color argument. It can be a color name (e.g., `blue`, `teal`, `orange`) or a hex value (e.g., `#3B82F6`). If omitted, a random color is generated.

---

## DO NOT ACTIVATE

This skill should NOT be invoked when:
- User mentions "VS Code" or "settings" in casual conversation
- User asks about VS Code extensions or features generally
- User discusses color themes or appearance preferences without invoking the command
- Any context where `/vsc-tricks` was not explicitly typed

Only activate on explicit `/vsc-tricks` command invocation.
