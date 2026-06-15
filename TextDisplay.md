# TextDisplay

`TextDisplayBuilder` renders markdown text inside a CV2 layout. It's the primary way to show any written content — titles, descriptions, field-like info, etc. It directly replaces the old `content` field and embed descriptions.

---

## Basic Usage

```js
const { TextDisplayBuilder, MessageFlags } = require('discord.js');

const text = new TextDisplayBuilder()
  .setContent('Hello, world!');

await channel.send({
  components: [text],
  flags: MessageFlags.IsComponentsV2,
});
```

---

## Markdown is Fully Supported

```js
new TextDisplayBuilder().setContent('### Ticket Opened')
new TextDisplayBuilder().setContent('**Status:** Open')
new TextDisplayBuilder().setContent('>>> This is a blockquote')
new TextDisplayBuilder().setContent('`inline code` works too')
new TextDisplayBuilder().setContent('-# Small subtext at the bottom')
```

---

## Multi-line Text

```js
new TextDisplayBuilder().setContent(
  '**User:** John\n' +
  '**Category:** Support\n' +
  '**Created:** <t:1700000000:R>'
)
```

---

## Discord Timestamps

```js
const ts = Math.floor(Date.now() / 1000);

new TextDisplayBuilder().setContent(`Opened <t:${ts}:R>`)  // "5 minutes ago"
new TextDisplayBuilder().setContent(`Opened <t:${ts}:F>`)  // "Monday, 1 January 2024 12:00"
```

---

## User & Channel Mentions

Mentions inside `TextDisplayBuilder` **will notify users and roles** — use `allowedMentions` to control this:

```js
await channel.send({
  components: [
    new TextDisplayBuilder().setContent(`Created by <@${user.id}>`)
  ],
  flags: MessageFlags.IsComponentsV2,
  allowedMentions: { users: [] }, // suppress the ping
});
```

---

## Setting a Component ID

```js
const DISPLAY_ID = 1001;

new TextDisplayBuilder()
  .setContent('Count: 0')
  .setId(DISPLAY_ID)
```

Use `.setId()` so you can find and update this component later via interaction responses.

---

## Notes

- You can have as many `TextDisplayBuilder` items as needed
- No title/description split like in embeds — use markdown headings (`#`, `##`, `###`)
- Total text across **all** `TextDisplayBuilder` components in one message cannot exceed **4000 characters**
- Emojis (unicode and custom Discord ones) render fine
