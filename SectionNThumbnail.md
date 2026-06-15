# Section & Thumbnail

`SectionBuilder` places text **side-by-side** with an accessory — either a `ThumbnailBuilder` (image) or a `ButtonBuilder`. Useful for profile cards, user info panels, or any text + image combo.

---

## With a Thumbnail (Image)

```js
const { SectionBuilder, ThumbnailBuilder, TextDisplayBuilder, MessageFlags } = require('discord.js');

const section = new SectionBuilder()
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent('**John Doe**'),
    new TextDisplayBuilder().setContent('Support ticket opened.')
  )
  .setThumbnailAccessory(
    new ThumbnailBuilder().setURL('https://example.com/avatar.png')
  );

await channel.send({
  components: [section],
  flags: MessageFlags.IsComponentsV2,
});
```

---

## With a User's Avatar

```js
const section = new SectionBuilder()
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent(`**${interaction.user.displayName}**`),
    new TextDisplayBuilder().setContent('Category: General')
  )
  .setThumbnailAccessory(
    new ThumbnailBuilder()
      .setURL(interaction.user.displayAvatarURL())
      .setDescription('User avatar') // alt text
  );
```

---

## With a Server Icon

```js
const iconUrl = interaction.guild.iconURL();

const section = new SectionBuilder()
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent(`**__${title}__**\n\n${description}`)
  )
  .setThumbnailAccessory(
    new ThumbnailBuilder().setURL(iconUrl)
  );
```

---

## How it Renders

```
┌─────────────────────────────────────────┐
│  Text content here                 [img] │
│  More text on the left side              │
└─────────────────────────────────────────┘
```

The thumbnail appears on the **right**, text on the **left**.

---

## Button as Accessory

A `ButtonBuilder` can also be the section accessory — placed on the right side next to your text:

```js
const { SectionBuilder, TextDisplayBuilder, ButtonBuilder, ButtonStyle } = require('discord.js');

const section = new SectionBuilder()
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent('Count: 0').setId(1001)
  )
  .setButtonAccessory(
    new ButtonBuilder()
      .setLabel('+1')
      .setStyle(ButtonStyle.Primary)
      .setCustomId('increment_btn')
  );
```

Useful for compact action + label combos without needing a full `ActionRowBuilder`.

---

## Spoiler Thumbnail

```js
new ThumbnailBuilder()
  .setURL('https://example.com/image.png')
  .setSpoiler(true) // blurs until clicked
```

---

## Notes

- A section can hold **1 to 3** `TextDisplayBuilder` items
- The accessory can be a `ThumbnailBuilder` **or** a `ButtonBuilder` — both are supported
- `ThumbnailBuilder` only accepts a URL — use `attachment://filename` for local files
- The section itself can be top-level or inside a `ContainerBuilder`
