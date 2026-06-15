# ActionRow

`ActionRowBuilder` lets you place **buttons and select menus** inside a CV2 layout. In CV2, action rows go inside a `ContainerBuilder` or at the top level of the `components` array.

---

## Buttons Inside a Container

```js
const { ContainerBuilder, ActionRowBuilder, ButtonBuilder, ButtonStyle, MessageFlags } = require('discord.js');

const container = new ContainerBuilder()
  .addActionRowComponents(
    new ActionRowBuilder().addComponents(
      new ButtonBuilder().setLabel('Confirm').setStyle(ButtonStyle.Success).setCustomId('confirm'),
      new ButtonBuilder().setLabel('Cancel').setStyle(ButtonStyle.Danger).setCustomId('cancel')
    )
  );
```

---

## Handling Button Clicks

```js
client.on('interactionCreate', async interaction => {
  if (!interaction.isButton()) return;

  if (interaction.customId === 'confirm') {
    await interaction.reply({ content: 'Confirmed!', ephemeral: true });
  }

  if (interaction.customId === 'cancel') {
    await interaction.reply({ content: 'Cancelled.', ephemeral: true });
  }
});
```

---

## Select Menu Inside a Container

```js
const { StringSelectMenuBuilder, StringSelectMenuOptionBuilder, ActionRowBuilder, ContainerBuilder, MessageFlags } = require('discord.js');

const select = new StringSelectMenuBuilder()
  .setCustomId('category_select')
  .setPlaceholder('Choose a category...')
  .addOptions(
    new StringSelectMenuOptionBuilder().setLabel('General').setValue('general'),
    new StringSelectMenuOptionBuilder().setLabel('Billing').setValue('billing')
  );

const container = new ContainerBuilder()
  .addActionRowComponents(
    new ActionRowBuilder().addComponents(select)
  );
```

---

## Up to 5 Buttons Per Row

```js
const buttons = categories.slice(0, 5).map(name =>
  new ButtonBuilder().setLabel(name).setStyle(ButtonStyle.Primary).setCustomId(`btn_${name}`)
);

const container = new ContainerBuilder()
  .addActionRowComponents(
    new ActionRowBuilder().addComponents(...buttons)
  );
```

For more than 5 buttons, add multiple action rows:

```js
for (let i = 0; i < buttons.length; i += 5) {
  container.addActionRowComponents(
    new ActionRowBuilder().addComponents(...buttons.slice(i, i + 5))
  );
}
```

---

## Button Styles

```js
ButtonStyle.Primary    // blurple
ButtonStyle.Secondary  // grey
ButtonStyle.Success    // green
ButtonStyle.Danger     // red
ButtonStyle.Link       // grey with external link icon (requires .setURL())
```

---

## Notes

- Each `ActionRow` holds up to **5 buttons** or **1 select menu**
- `custom_id` is required on all buttons and selects (except `ButtonStyle.Link`)
- Button interactions are handled via the `interactionCreate` event
