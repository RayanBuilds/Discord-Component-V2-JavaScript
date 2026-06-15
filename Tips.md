# Tips & Gotchas

Common mistakes, limits, and things to know before you ship.

---

## Always Pass the Flag

Every CV2 message **must** include `MessageFlags.IsComponentsV2`. Without it, display components won't render.

```js
await channel.send({
  components: [container],
  flags: MessageFlags.IsComponentsV2, // ← required every time
});
```

---

## Old Components Still Work

CV2 is **opt-in per message** — you don't have to migrate your whole bot. Old `ActionRow` + `View` style messages still work fine alongside CV2 in the same bot.

---

## No Embeds, Content, Stickers, or Polls Alongside CV2

When sending a CV2 message, you **cannot** include:
- `content`
- `embeds`
- `stickers`
- `poll`

```js
// Wrong
await channel.send({ content: 'Hello', components: [container], flags: MessageFlags.IsComponentsV2 });

// Right — CV2 handles everything
await channel.send({ components: [container], flags: MessageFlags.IsComponentsV2 });
```

> **Editing tip:** To convert a non-CV2 message to CV2, set `content: null` and `embeds: []` in the edit call.

---

## The Flag Can't Be Removed

Once a message is sent with `IsComponentsV2`, the flag **cannot be removed** from that message by editing it. You can only edit the component content.

---

## Containers Can't Be Nested

You cannot put a `ContainerBuilder` inside another `ContainerBuilder`:

```
components: [
  ContainerBuilder   ✅
  ContainerBuilder   ✅
    └── ContainerBuilder  ❌ not allowed
]
```

---

## attachment:// Filename Must Match Exactly

```js
const file = new AttachmentBuilder('./report.txt').setName('report.txt');
new FileBuilder().setURL('attachment://report.txt')  // ✅

new FileBuilder().setURL('attachment://Report.txt')  // ❌ case sensitive
```

Always call `.setName()` on `AttachmentBuilder` explicitly to avoid mismatches.

---

## Mentions Will Ping — Use allowedMentions

User and role mentions inside `TextDisplayBuilder` **will notify** those users/roles:

```js
await channel.send({
  components: [new TextDisplayBuilder().setContent(`Hey <@${userId}>`)],
  flags: MessageFlags.IsComponentsV2,
  allowedMentions: { users: [] }, // suppress pings
});
```

---

## Sections Allow 1–3 Text Displays

A `SectionBuilder` accepts between **1 and 3** `TextDisplayBuilder` items. More than 3 will cause an error.

---

## Component Limits

| Limit | Value |
|-------|-------|
| **Total components per message** | **40 (including nested)** |
| Text displays per section | 1–3 |
| Buttons per ActionRow | 5 |
| Select options | 25 |
| Items in MediaGallery | 10 |
| Total text across all TextDisplays | 4000 characters |

The **40-component limit** is the most important to watch — every `ContainerBuilder`, `TextDisplayBuilder`, `SeparatorBuilder`, `SectionBuilder`, `ActionRowBuilder`, `ButtonBuilder`, etc. counts toward it, including nested ones.
