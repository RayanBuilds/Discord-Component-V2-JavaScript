# Buttons

Buttons are interactive components placed inside an `ActionRowBuilder`. When clicked, they fire an `interactionCreate` event that your bot handles.

---

## Button Styles

```js
const { ButtonBuilder, ButtonStyle } = require('discord.js');

new ButtonBuilder().setLabel('Primary').setStyle(ButtonStyle.Primary).setCustomId('btn_primary')
new ButtonBuilder().setLabel('Secondary').setStyle(ButtonStyle.Secondary).setCustomId('btn_secondary')
new ButtonBuilder().setLabel('Success').setStyle(ButtonStyle.Success).setCustomId('btn_success')
new ButtonBuilder().setLabel('Danger').setStyle(ButtonStyle.Danger).setCustomId('btn_danger')
new ButtonBuilder().setLabel('Link').setStyle(ButtonStyle.Link).setURL('https://discord.gg/aerox')
```

> `ButtonStyle.Link` opens a URL — it does **not** fire an interaction. No `custom_id` needed.

---

## Basic Button in a CV2 Message

```js
const { ContainerBuilder, TextDisplayBuilder, ActionRowBuilder, ButtonBuilder, ButtonStyle, MessageFlags } = require('discord.js');

const container = new ContainerBuilder()
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent('Press the button below.')
  )
  .addActionRowComponents(
    new ActionRowBuilder().addComponents(
      new ButtonBuilder()
        .setLabel('Click Me')
        .setStyle(ButtonStyle.Primary)
        .setCustomId('my_button')
    )
  );

await interaction.reply({
  components: [container],
  flags: MessageFlags.IsComponentsV2,
});
```

---

## Handling the Click

```js
client.on('interactionCreate', async interaction => {
  if (!interaction.isButton()) return;

  if (interaction.customId === 'my_button') {
    await interaction.reply({ content: 'You clicked it!', ephemeral: true });
  }
});
```

---

## Multiple Buttons in One Row

```js
new ActionRowBuilder().addComponents(
  new ButtonBuilder().setLabel('Accept').setStyle(ButtonStyle.Success).setCustomId('accept'),
  new ButtonBuilder().setLabel('Decline').setStyle(ButtonStyle.Danger).setCustomId('decline'),
  new ButtonBuilder().setLabel('Info').setStyle(ButtonStyle.Secondary).setCustomId('info')
)
```

Up to **5 buttons** per row. For more, add another `ActionRowBuilder`.

---

## Disabled Button

```js
new ButtonBuilder()
  .setLabel('Unavailable')
  .setStyle(ButtonStyle.Secondary)
  .setCustomId('disabled_btn')
  .setDisabled(true)
```

---

## Button with an Emoji

```js
new ButtonBuilder()
  .setLabel('Approve')
  .setStyle(ButtonStyle.Success)
  .setCustomId('approve')
  .setEmoji('✅')

// Custom Discord emoji:
new ButtonBuilder()
  .setLabel('Boost')
  .setStyle(ButtonStyle.Primary)
  .setCustomId('boost')
  .setEmoji({ id: '1234567890', name: 'boost' })
```

---

## Button as Section Accessory

A button can sit **inline next to text** inside a `SectionBuilder` — no `ActionRow` needed:

```js
const { SectionBuilder, TextDisplayBuilder, ButtonBuilder, ButtonStyle } = require('discord.js');

const section = new SectionBuilder()
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent('Open a support ticket using the button →')
  )
  .setButtonAccessory(
    new ButtonBuilder()
      .setLabel('Open Ticket')
      .setStyle(ButtonStyle.Primary)
      .setCustomId('open_ticket')
  );
```

---

## Updating the Message After a Click

```js
client.on('interactionCreate', async interaction => {
  if (!interaction.isButton() || interaction.customId !== 'confirm') return;

  const confirmed = new ContainerBuilder()
    .addTextDisplayComponents(
      new TextDisplayBuilder().setContent('✅ Confirmed!')
    );

  await interaction.update({
    components: [confirmed],
    flags: MessageFlags.IsComponentsV2,
  });
});
```

---

## Persistent Buttons (Survive Bot Restarts)

For buttons that need to keep working after the bot goes offline, always set a `custom_id` and make sure your `interactionCreate` handler is registered on startup — Discord.js re-attaches them automatically since it listens globally.

```js
// As long as your interactionCreate handler runs on startup, persistent buttons work.
client.on('interactionCreate', async interaction => {
  if (!interaction.isButton()) return;
  // handle by customId...
});
```

---

## Notes

- Each `ActionRow` holds up to **5 buttons**
- `custom_id` is required on all buttons except `ButtonStyle.Link`
- `custom_id` can be up to **100 characters** — use it to store state (e.g. `close_ticket_42`)
- Buttons time out after **15 minutes** unless you handle them as persistent
