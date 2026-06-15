# Message Structure

In Discord.js, there is no `LayoutView` class. A Components V2 message is just a regular `send()` or `reply()` call with two key things:

1. A `components` array containing your builders
2. The `MessageFlags.IsComponentsV2` flag

---

## Basic Structure

```js
const { MessageFlags } = require('discord.js');

await channel.send({
  components: [/* your components here */],
  flags: MessageFlags.IsComponentsV2,
});
```

---

## Replying to an Interaction

```js
await interaction.reply({
  components: [container],
  flags: MessageFlags.IsComponentsV2,
});
```

---

## Ephemeral Response

```js
await interaction.reply({
  components: [container],
  flags: MessageFlags.IsComponentsV2 | MessageFlags.Ephemeral,
});
```

---

## Following Up After Defer

```js
await interaction.deferReply();

// ... do async work ...

await interaction.editReply({
  components: [container],
  flags: MessageFlags.IsComponentsV2,
});
```

> You do **not** need the flag when deferring — only when sending the actual component content.

---

## Multiple Top-Level Components

You can pass multiple builders in the array — they stack vertically:

```js
await channel.send({
  components: [textDisplay, separator, container],
  flags: MessageFlags.IsComponentsV2,
});
```

---

## How It Fits Together

```
components: [
  ContainerBuilder
  ├── TextDisplayBuilder
  ├── SeparatorBuilder
  ├── SectionBuilder
  │   ├── TextDisplayBuilder
  │   └── ThumbnailBuilder (accessory)
  ├── MediaGalleryBuilder
  └── ActionRowBuilder
      └── ButtonBuilder
]
```

---

## Component `id` vs `custom_id`

Every component can have an optional numeric `id` field — **not** the same as `custom_id`.

```js
new TextDisplayBuilder().setContent('Count: 0').setId(1001)
```

- `id` — a number you set to find a component later in interaction responses
- `custom_id` — a string on buttons/selects that triggers interaction events

---

## Notes

- The `IsComponentsV2` flag **cannot be removed** from a message once sent
- Once set, `content`, `embeds`, `stickers`, and `polls` are blocked on that message
- Old `components: [actionRow]` style messages still work — CV2 is opt-in per message
- Total component limit: **40** (including nested)
- Total text limit: **4000 characters** across all `TextDisplayBuilder` content
