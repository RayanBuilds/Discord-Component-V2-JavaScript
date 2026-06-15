# Container

`ContainerBuilder` is the main building block of a CV2 layout. It groups components together and optionally adds a colored left-side accent bar — similar to the color strip on a Discord embed.

---

## Basic Usage

```js
const { ContainerBuilder, TextDisplayBuilder, MessageFlags } = require('discord.js');

const container = new ContainerBuilder()
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent('Hello!')
  );

await channel.send({
  components: [container],
  flags: MessageFlags.IsComponentsV2,
});
```

---

## With an Accent Color

```js
const container = new ContainerBuilder()
  .setAccentColor(0x5865F2); // Discord blurple
```

Pass any hex color as a number:

```js
new ContainerBuilder().setAccentColor(0x57F287) // green
new ContainerBuilder().setAccentColor(0xED4245) // red
new ContainerBuilder().setAccentColor(0xFF8C00) // orange
```

---

## No Color (Default)

Simply don't call `.setAccentColor()` — the container renders as a plain outlined box with no accent bar.

---

## Spoiler Container

You can blur the entire container until the user clicks it:

```js
const container = new ContainerBuilder()
  .setSpoiler(true)
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent('Hidden content!')
  );
```

---

## Adding Multiple Components

```js
const { ContainerBuilder, TextDisplayBuilder, SeparatorBuilder, SeparatorSpacingSize } = require('discord.js');

const container = new ContainerBuilder()
  .setAccentColor(0x5865F2)
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent('### Title')
  )
  .addSeparatorComponents(
    new SeparatorBuilder().setDivider(true).setSpacing(SeparatorSpacingSize.Small)
  )
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent('Some body text here.')
  );
```

---

## Notes

- A message can hold **multiple** containers at the top level
- Containers can hold: `TextDisplayBuilder`, `SeparatorBuilder`, `SectionBuilder`, `MediaGalleryBuilder`, `FileBuilder`, `ActionRowBuilder`
- Containers **cannot** be nested inside other containers
