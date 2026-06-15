# Discord.js Components V2 — Beginner Friendly Guide

> Guide by **RayanBuilds**

Components V2 is Discord's new message layout system that gives you **full control** over how your bot messages look. It's more powerful and flexible than the traditional `embed` system.

---

## What's New in Components V2?

- Fully customizable layouts
- Rich text with full markdown support
- Clean UI with sections, images, and spacing
- Buttons & dropdowns work right inside the layout
- `content`, `embeds`, `stickers`, and `polls` **cannot** be used alongside CV2 components
- Must pass `MessageFlags.IsComponentsV2` flag on every CV2 message

---

## Basic Building Blocks

| Builder | What It Does |
|---------|-------------|
| `ContainerBuilder` | Outer layout block with optional accent color |
| `TextDisplayBuilder` | Renders text with full markdown support |
| `SectionBuilder` | Groups text side-by-side with a thumbnail or button |
| `SeparatorBuilder` | Adds a divider line or spacing between components |
| `MediaGalleryBuilder` | Displays images/videos in a grid |
| `FileBuilder` | Attaches and shows files inline |
| `ActionRowBuilder` | Adds buttons or select menus inside a layout |
| `ThumbnailBuilder` | Small image accessory inside a Section |

---

## Simple Example

```js
const { ContainerBuilder, TextDisplayBuilder, MessageFlags } = require('discord.js');

const container = new ContainerBuilder()
  .setAccentColor(0x5865F2)
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent('# Welcome!\n**Click the button below.**')
  );

await interaction.reply({
  components: [container],
  flags: MessageFlags.IsComponentsV2,
});
```

---

## Button Click Handling

```js
const { SectionBuilder, TextDisplayBuilder, ButtonBuilder, ButtonStyle, MessageFlags } = require('discord.js');

const section = new SectionBuilder()
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent('Click the button to continue.')
  )
  .setButtonAccessory(
    new ButtonBuilder()
      .setLabel('Say Hi')
      .setStyle(ButtonStyle.Primary)
      .setCustomId('hello_btn')
  );

// In your interactionCreate handler:
client.on('interactionCreate', async i => {
  if (!i.isButton()) return;
  if (i.customId === 'hello_btn') {
    await i.reply({ content: 'Hi! 👋', ephemeral: true });
  }
});
```

---

## Visual Layout Example (Dashboard Style)

```js
const { ContainerBuilder, TextDisplayBuilder, SeparatorBuilder, SeparatorSpacingSize, SectionBuilder, ButtonBuilder, ButtonStyle, MessageFlags } = require('discord.js');

const container = new ContainerBuilder()
  .setAccentColor(0x2F3136)
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent('# Server Stats')
  )
  .addSeparatorComponents(
    new SeparatorBuilder().setDivider(true).setSpacing(SeparatorSpacingSize.Small)
  )
  .addSectionComponents(
    new SectionBuilder()
      .addTextDisplayComponents(
        new TextDisplayBuilder().setContent('**Online Members:** 1,234'),
        new TextDisplayBuilder().setContent('**Messages Today:** 5,678')
      )
      .setButtonAccessory(
        new ButtonBuilder()
          .setCustomId('refresh_stats')
          .setLabel('Refresh')
          .setStyle(ButtonStyle.Secondary)
      )
  );

await interaction.reply({
  components: [container],
  flags: MessageFlags.IsComponentsV2,
});
```

---

## Showing Images

```js
const { MediaGalleryBuilder, MediaGalleryItemBuilder, MessageFlags } = require('discord.js');

const gallery = new MediaGalleryBuilder()
  .addItems(
    new MediaGalleryItemBuilder().setURL('https://example.com/image1.png').setDescription('Image 1'),
    new MediaGalleryItemBuilder().setURL('https://example.com/image2.png').setDescription('Image 2')
  );

await channel.send({
  components: [gallery],
  flags: MessageFlags.IsComponentsV2,
});
```

---

## Migrating from Embeds

| Old Embed | New CV2 Equivalent |
|-----------|-------------------|
| `embed.setTitle()` | `new TextDisplayBuilder().setContent('# Title')` |
| `embed.setDescription()` | `new TextDisplayBuilder().setContent('...')` |
| `embed.addFields()` | `SectionBuilder` with text displays |
| `embed.setImage()` | `MediaGalleryBuilder` |
| `embed.setThumbnail()` | `SectionBuilder.setThumbnailAccessory()` |
| `embed.setColor()` | `ContainerBuilder.setAccentColor()` |

---

## Important Tips

- **Always pass this flag** on every CV2 message: `flags: MessageFlags.IsComponentsV2`
- Max **40 components** per message (nested components count too)
- Max **4000 characters** across all `TextDisplayBuilder` content in one message
- Once a message is sent with the CV2 flag, it **cannot be removed** from that message
- Old `MessageComponent` style still works — CV2 is opt-in per message
- Test your layouts on mobile too
- Use `SeparatorBuilder` for spacing and visual breaks

---

## Advanced: Pagination Example

```js
function buildPage(items, page = 0, pageSize = 5) {
  const chunk = items.slice(page * pageSize, (page + 1) * pageSize);

  const container = new ContainerBuilder()
    .addTextDisplayComponents(
      new TextDisplayBuilder().setContent(`# Page ${page + 1}`)
    )
    .addSeparatorComponents(
      new SeparatorBuilder().setDivider(true).setSpacing(SeparatorSpacingSize.Small)
    );

  chunk.forEach(item => {
    container.addTextDisplayComponents(
      new TextDisplayBuilder().setContent(`**${item.title}**\n${item.desc}`)
    );
  });

  const buttons = [];
  if (page > 0)
    buttons.push(new ButtonBuilder().setCustomId('prev').setLabel('Prev').setStyle(ButtonStyle.Secondary));
  if ((page + 1) * pageSize < items.length)
    buttons.push(new ButtonBuilder().setCustomId('next').setLabel('Next').setStyle(ButtonStyle.Secondary));

  if (buttons.length)
    container.addActionRowComponents(new ActionRowBuilder().addComponents(...buttons));

  return container;
}
```

---

## Table of Contents

| # | Guide | Description |
|---|-------|-------------|
| 1 | [Message Structure](LayoutView.md) | How CV2 messages are structured |
| 2 | [Container](Container.md) | Grouping components with optional accent color |
| 3 | [TextDisplay](TextDisplay.md) | Rendering markdown text |
| 4 | [Separator](Separator.md) | Visual dividers and spacing |
| 5 | [Section & Thumbnail](SectionNThumbnail.md) | Side-by-side text + image or button |
| 6 | [MediaGallery](MediaGallery.md) | Image and video grids |
| 7 | [File Component](FileComponent.md) | Inline file attachments |
| 8 | [ActionRow](ActionRow.md) | Buttons and selects inside CV2 |
| 9 | [Real-World Examples](ExampleUsage.md) | Full patterns from a production bot |
| 10 | [Tips & Gotchas](Tips.md) | Limits, mistakes, and best practices |
