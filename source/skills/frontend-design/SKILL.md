---
name: frontend-design
description: "DEPRECATED: This skill has been renamed to 'venustus'. Run /venustus instead."
---

This skill has been renamed to **venustus**. All design functionality now lives in the `{{command_prefix}}venustus` skill.

Check if the venustus skill is available:
- If `{{command_prefix}}venustus` exists, use it directly. You can safely inform the user that the `frontend-design` folder is a deprecated leftover and can be deleted.
- If `{{command_prefix}}venustus` does NOT exist, tell the user to update their skills by running `npx venustas skills update` in their terminal.

Do NOT attempt any design work from this skill. Redirect to `{{command_prefix}}venustus` instead.
