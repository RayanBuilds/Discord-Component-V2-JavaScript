# Real-World Examples

Complete, working patterns for common Discord.js CV2 use cases.

---

## 1. Simple Ephemeral Notification

The most common pattern — send a short message back to the user:

```js
const { ContainerBuilder, TextDisplayBuilder, MessageFlags } = require('discord.js');

const container = new ContainerBuilder()
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent('You are blacklisted from creating tickets.')
  );

await interaction.reply({
  components: [container],
  flags: MessageFlags.IsComponentsV2 | MessageFlags.Ephemeral,
});
```

---

## 2. Info Container with Title + Body

The standard embed replacement pattern:

```js
const { ContainerBuilder, TextDisplayBuilder, SeparatorBuilder, SeparatorSpacingSize, MessageFlags } = require('discord.js');

const container = new ContainerBuilder()
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent('### Ticket Reopened')
  )
  .addSeparatorComponents(
    new SeparatorBuilder().setDivider(true).setSpacing(SeparatorSpacingSize.Small)
  )
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent(`Reopened by <@${interaction.user.id}>`)
  );

await interaction.reply({
  components: [container],
  flags: MessageFlags.IsComponentsV2,
});
```

---

## 3. Ticket Control Panel (Section + Buttons)

A full ticket panel with avatar thumbnail and action buttons:

```js
const container = new ContainerBuilder()
  .setAccentColor(0x5865F2)
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent('# Support Ticket')
  )
  .addSeparatorComponents(
    new SeparatorBuilder().setDivider(true).setSpacing(SeparatorSpacingSize.Small)
  )
  .addSectionComponents(
    new SectionBuilder()
      .addTextDisplayComponents(
        new TextDisplayBuilder().setContent(
          `**Welcome** <@${user.id}>\n**Category:** ${category}\n\nOur team will assist you shortly.`
        )
      )
      .setThumbnailAccessory(
        new ThumbnailBuilder().setURL(user.displayAvatarURL())
      )
  )
  .addSeparatorComponents(
    new SeparatorBuilder().setDivider(true).setSpacing(SeparatorSpacingSize.Small)
  )
  .addActionRowComponents(
    new ActionRowBuilder().addComponents(
      new ButtonBuilder().setLabel('Claim').setStyle(ButtonStyle.Primary).setCustomId('ticket_claim'),
      new ButtonBuilder().setLabel('Close').setStyle(ButtonStyle.Danger).setCustomId('ticket_close')
    )
  );

await channel.send({
  components: [container],
  flags: MessageFlags.IsComponentsV2,
});
```

---

## 4. Panel with Dropdown Select

A ticket panel using a dropdown to pick a category:

```js
const options = categories.map(name =>
  new StringSelectMenuOptionBuilder().setLabel(name).setValue(name)
);

const container = new ContainerBuilder()
  .setAccentColor(0x5865F2)
  .addSectionComponents(
    new SectionBuilder()
      .addTextDisplayComponents(
        new TextDisplayBuilder().setContent(`**__${title}__**\n\n${description}`)
      )
      .setThumbnailAccessory(
        new ThumbnailBuilder().setURL(interaction.guild.iconURL())
      )
  )
  .addSeparatorComponents(
    new SeparatorBuilder().setDivider(true).setSpacing(SeparatorSpacingSize.Small)
  )
  .addActionRowComponents(
    new ActionRowBuilder().addComponents(
      new StringSelectMenuBuilder()
        .setCustomId('ticket_category_select')
        .setPlaceholder('Select a category...')
        .addOptions(...options)
    )
  );

await channel.send({
  components: [container],
  flags: MessageFlags.IsComponentsV2,
});
```

---

## 5. Log Message with File Attachment

Sending a log to a staff channel including a file:

```js
const filename = `ticket-${String(ticketNumber).padStart(4, '0')}-transcript.txt`;
const file = new AttachmentBuilder('./transcripts/' + filename).setName(filename);

const container = new ContainerBuilder()
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent('### Logs - Ticket Deleted')
  )
  .addSeparatorComponents(
    new SeparatorBuilder().setDivider(true).setSpacing(SeparatorSpacingSize.Small)
  )
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent(
      `Ticket \`#${String(ticketNumber).padStart(4, '0')}\` deleted by ${interaction.user.displayName}\n**Category:** ${category}`
    )
  )
  .addSeparatorComponents(
    new SeparatorBuilder().setDivider(true).setSpacing(SeparatorSpacingSize.Small)
  )
  .addFileComponents(
    new FileBuilder().setURL(`attachment://${filename}`)
  );

await logChannel.send({
  components: [container],
  files: [file],
  flags: MessageFlags.IsComponentsV2,
});
```

---

## 6. Avatar Display with MediaGallery

Showing a user's avatar inside a container:

```js
const avatarUrl = user.displayAvatarURL({ size: 1024, extension: 'png' });

const container = new ContainerBuilder()
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent(`### ${user.displayName}'s Avatar`)
  )
  .addSeparatorComponents(
    new SeparatorBuilder().setDivider(true).setSpacing(SeparatorSpacingSize.Small)
  )
  .addMediaGalleryComponents(
    new MediaGalleryBuilder().addItems(
      new MediaGalleryItemBuilder().setURL(avatarUrl).setDescription('User avatar')
    )
  )
  .addSeparatorComponents(
    new SeparatorBuilder().setDivider(false).setSpacing(SeparatorSpacingSize.Small)
  )
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent(`[PNG](${avatarUrl}) | [WebP](${user.displayAvatarURL({ extension: 'webp' })})`)
  );

await interaction.reply({
  components: [container],
  flags: MessageFlags.IsComponentsV2 | MessageFlags.Ephemeral,
});
```

---

## 7. Full Slash Command — Everything Together

```js
const { SlashCommandBuilder, MessageFlags, ContainerBuilder, TextDisplayBuilder,
  SeparatorBuilder, SeparatorSpacingSize, SectionBuilder, ThumbnailBuilder,
  MediaGalleryBuilder, MediaGalleryItemBuilder, ActionRowBuilder, ButtonBuilder, ButtonStyle } = require('discord.js');

module.exports = {
  data: new SlashCommandBuilder().setName('showcase').setDescription('CV2 showcase'),

  async execute(interaction) {
    const avatar = interaction.client.user.displayAvatarURL({ extension: 'png', size: 512 });

    const container = new ContainerBuilder()
      .setAccentColor(0x5865F2)
      .addTextDisplayComponents(
        new TextDisplayBuilder().setContent('# Components V2 Showcase')
      )
      .addSeparatorComponents(
        new SeparatorBuilder().setDivider(true).setSpacing(SeparatorSpacingSize.Small)
      )
      .addSectionComponents(
        new SectionBuilder()
          .addTextDisplayComponents(
            new TextDisplayBuilder().setContent('**Bot Info**'),
            new TextDisplayBuilder().setContent('Thumbnail next to some text.')
          )
          .setThumbnailAccessory(
            new ThumbnailBuilder().setURL(avatar).setDescription('Bot avatar')
          )
      )
      .addSeparatorComponents(
        new SeparatorBuilder().setDivider(true).setSpacing(SeparatorSpacingSize.Small)
      )
      .addMediaGalleryComponents(
        new MediaGalleryBuilder().addItems(
          new MediaGalleryItemBuilder().setURL('https://picsum.photos/seed/a/400/300').setDescription('Image 1'),
          new MediaGalleryItemBuilder().setURL('https://picsum.photos/seed/b/400/300').setDescription('Image 2')
        )
      )
      .addSeparatorComponents(
        new SeparatorBuilder().setDivider(true).setSpacing(SeparatorSpacingSize.Small)
      )
      .addActionRowComponents(
        new ActionRowBuilder().addComponents(
          new ButtonBuilder().setLabel('Click Me').setStyle(ButtonStyle.Primary).setCustomId('demo_btn')
        )
      )
      .addTextDisplayComponents(
        new TextDisplayBuilder().setContent('-# Powered by Components V2')
      );

    await interaction.reply({
      components: [container],
      flags: MessageFlags.IsComponentsV2,
    });
  },
};
```
