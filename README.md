# pi-ask-user-question

A small pi package that registers an `ask_user_question` tool for focused user input.

It is forked from a local `~/.pi/agent/extensions/ask-user-question.ts` extension and adds a path for RPC clients such as Pendant / VS Code: rich terminal UI still uses `ctx.ui.custom(...)`, while RPC mode uses standard `ctx.ui.select(...)`, `ctx.ui.input(...)`, and `ctx.ui.editor(...)` calls so clients can answer through the extension UI protocol.

## Install

```bash
pi install git:github.com/fractiunate/pi-ask-user-question
```

Or test without installing:

```bash
pi -e git:github.com/fractiunate/pi-ask-user-question
```

## Tool

`ask_user_question`

Parameters:

```json
{
  "question": "Which option should we use?",
  "details": "Optional context shown to the user.",
  "options": [
    { "label": "Option A", "value": "a", "description": "Short detail" },
    { "label": "Option B", "value": "b" }
  ],
  "multiSelect": false
}
```

- Omit `options` for free text.
- With options, `Other` is always available.
- Set `multiSelect: true` to allow multiple answers.

## Pendant / RPC behavior

Pi RPC mode emits `extension_ui_request` events for standard UI methods. Pendant or another RPC client should answer with matching `extension_ui_response` messages.

This extension avoids `ctx.ui.custom(...)` in RPC mode because pi documents that `custom()` is TUI-only and returns `undefined` in RPC.
