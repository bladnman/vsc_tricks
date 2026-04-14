# Colorize

Set random or user-specified window border and title bar colors for the current project's VS Code workspace.

---

## Before You Start

1. Check if `.vscode/settings.json` exists in the **target project root** (the directory where the user invoked the command — NOT this skill's own directory)
2. If it exists, read it — you'll merge color settings into the existing file
3. If it doesn't exist, you'll create `.vscode/` and `settings.json` from scratch

---

## Philosophy

**The goal is visual project identity.** Each project window gets a distinct border/title bar color so the user can instantly tell which project they're looking at. This is purely cosmetic — no themes, no icon packs, no functional settings changes.

- Do NOT set `workbench.colorTheme` or `workbench.iconTheme`
- Do NOT touch any settings outside of `workbench.colorCustomizations`
- Do NOT overwrite non-color settings if `settings.json` already has content

---

## Process

### 1. Determine the Color

**If the user provided a hex value** (e.g., `#3B82F6`):
- Use it directly as the active color

**If the user provided a color name** (e.g., `blue`, `teal`, `coral`, `orange`):
- Pick a vibrant, saturated shade in that color family
- Aim for a hue that looks good as a thin border and title bar — not too dark, not too pastel
- Use your judgment to select a specific hex within the family

**If no color was provided** (random mode):
- Generate a random hue (0–360)
- Use high saturation (65–85%) and medium-high lightness (45–60%) to ensure the color is vivid and recognizable
- Convert HSL to hex
- Avoid muddy browns, near-grays, and near-whites — these defeat the purpose

### 2. Derive the Inactive Variant

The inactive color should be the **same hue** as the active color, but visibly dimmer:

- **Method:** Append a transparency suffix to the hex (e.g., `B3` for ~70% opacity) OR reduce saturation and lightness
- **Preferred approach:** Use the same hex with alpha `B3` appended (e.g., `#3B82F6` → `#3B82F6B3`)
- The result should clearly read as "same color, less prominent"

### 3. Build the Settings

Apply these `workbench.colorCustomizations` keys:

```json
{
  "workbench.colorCustomizations": {
    "window.activeBorder": "#<active_hex>",
    "window.inactiveBorder": "#<active_hex>B3",
    "titleBar.activeBackground": "#<active_hex>",
    "titleBar.activeForeground": "#FFFFFF or #000000 based on contrast",
    "titleBar.inactiveBackground": "#<active_hex>B3",
    "titleBar.inactiveForeground": "#FFFFFFB3 or #000000B3 based on contrast"
  }
}
```

**Foreground contrast rule:** If the active color is dark (perceived luminance < 0.5), use white (`#FFFFFF`) foreground. If light, use black (`#000000`). Use the relative luminance formula: `L = 0.2126*R + 0.7152*G + 0.0722*B` (with R/G/B normalized to 0–1).

### 4. Write the File

- If `.vscode/settings.json` exists: **merge** the `workbench.colorCustomizations` block into the existing JSON, preserving all other settings
- If it doesn't exist: create `.vscode/settings.json` with just the color customizations block
- Format the JSON with 2-space indentation

### 5. Report to the User

After writing, tell the user:

- The color you chose (hex value)
- Whether it was random, from a color name, or an explicit hex
- That they can run `/vsc-tricks colorize` again for a different random color, or `/vsc-tricks colorize <color>` to pick one

Keep it to 2–3 lines. Don't over-explain.

---

## Output

The only output is the written/updated `.vscode/settings.json` in the target project and a brief confirmation message.
