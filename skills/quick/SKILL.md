---
name: quick
description: Use when the user wants a fast, scannable answer instead of prose — a recap, TL;DR, lightweight scan, "the short version," or "just the headlines" — and ALWAYS when their message starts with "/quick". Triggers include "quick recap", "give me the short version", "list the steps left", "scan this file", or any request where brevity matters more than explanation.
---

# Quick

## Overview

Give the user a fast, scannable answer. No preamble, no explanation, no padding — just a tight numbered list of short headlines.

## When to Use

Use this skill when either is true:

- The user's message starts with `/quick`. Everything after `/quick` is the request.
- The user clearly wants a lightweight, brief response (a recap, a TL;DR, a scan, "just the headlines," "the short version").

## Behavior

- Respond only with a numbered list of short headlines.
- Each item should be concise and action-oriented.
- Do not include explanations unless the user explicitly asks for detail.
- Do not include code blocks unless the user explicitly asks for code.
- Prefer 3–7 items.
- If the request is about a specific file, inspect only the most recently uploaded or discussed file in the conversation, unless the user asks for broader context.
- Do not add an intro line, a summary, or a closing remark. The list is the whole response.

## Response format

```
1. Headline
2. Headline
3. Headline
```

## Examples

User:
```
/quick recap the steps remaining
```
Response:
```
1. Finish the failing test
2. Update the edge case handling
3. Run the full test suite
4. Open the PR
```

User:
```
/quick list all the variables in the current file
```
Response:
```
1. userId
2. eventType
3. startDate
4. timezone
```
