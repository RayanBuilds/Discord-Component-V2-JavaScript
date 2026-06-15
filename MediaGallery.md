# MediaGallery

`MediaGalleryBuilder` displays one or more images or videos in a grid. It's the CV2 replacement for `embed.setImage()` and attachment previews.

---

## Single Image

```js
const { MediaGalleryBuilder, MediaGalleryItemBuilder, MessageFlags } = require('discord.js');

const gallery = new MediaGalleryBuilder()
  .addItems(
    new MediaGalleryItemBuilder().setURL('https://example.com/banner.png')
  );

await channel.send({
  components: [gallery],
  flags: MessageFlags.IsComponentsV2,
});
```

---

## Multiple Images (Grid)

```js
const gallery = new MediaGalleryBuilder()
  .addItems(
    new MediaGalleryItemBuilder().setURL('https://example.com/image1.png').setDescription('Image 1'),
    new MediaGalleryItemBuilder().setURL('https://example.com/image2.png').setDescription('Image 2'),
    new MediaGalleryItemBuilder().setURL('https://example.com/image3.png').setDescription('Image 3')
  );
```

Multiple images render as a tiled grid in Discord.

---

## With Alt Text and Spoiler

```js
new MediaGalleryItemBuilder()
  .setURL('https://example.com/image.png')
  .setDescription('A screenshot of the dashboard') // alt text
  .setSpoiler(true) // blurs until clicked
```

---

## Using a Local File

```js
const { AttachmentBuilder } = require('discord.js');

const file = new AttachmentBuilder('./assets/banner.png').setName('banner.png');

const gallery = new MediaGalleryBuilder()
  .addItems(
    new MediaGalleryItemBuilder().setURL('attachment://banner.png')
  );

await channel.send({
  components: [gallery],
  files: [file],
  flags: MessageFlags.IsComponentsV2,
});
```

---

## User Avatar Example

```js
const gallery = new MediaGalleryBuilder()
  .addItems(
    new MediaGalleryItemBuilder()
      .setURL(interaction.user.displayAvatarURL({ size: 512 }))
      .setDescription(`${interaction.user.username}'s avatar`)
  );
```

---

## Inside a Container

Galleries are usually placed after a separator for clean visual separation:

```js
const container = new ContainerBuilder()
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent('### Screenshots')
  )
  .addSeparatorComponents(
    new SeparatorBuilder().setDivider(true).setSpacing(SeparatorSpacingSize.Small)
  )
  .addMediaGalleryComponents(
    new MediaGalleryBuilder().addItems(
      new MediaGalleryItemBuilder().setURL(imageUrl)
    )
  );
```

---

## Notes

- Up to **10 items** per gallery
- Supports images, videos, and GIFs via URL
- Use `attachment://filename` for local files alongside `files: [attachment]`
