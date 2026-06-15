# Select Menus

Select menus let users pick one or more options from a dropdown. They go inside an `ActionRowBuilder` — one select per row.

---

## Types of Select Menus

| Builder | What It Does |
|---------|-------------|
| `StringSelectMenuBuilder` | Custom list of options you define |
| `UserSelectMenuBuilder` | Pick a user from the server |
| `RoleSelectMenuBuilder` | Pick a role |
| `ChannelSelectMenuBuilder` | Pick a channel |
| `MentionableSelectMenuBuilder` | Pick a user or role |

---

## String Select Menu

The most common type — you define the options yourself:

```js
const { StringSelectMenuBuilder, StringSelectMenuOptionBuilder, ActionRowBuilder, ContainerBuilder, TextDisplayBuilder, MessageFlags } = require('discord.js');

const select = new StringSelectMenuBuilder()
  .setCustomId('category_select')
  .setPlaceholder('Choose a category...')
  .addOptions(
    new StringSelectMenuOptionBuilder().setLabel('General').setValue('general').setDescription('General support'),
    new StringSelectMenuOptionBuilder().setLabel('Billing').setValue('billing').setDescription('Payment issues'),
    new StringSelectMenuOptionBuilder().setLabel('Technical').setValue('technical').setDescription('Technical help')
  );

const container = new ContainerBuilder()
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent('Select a support category below.')
  )
  .addActionRowComponents(
    new ActionRowBuilder().addComponents(select)
  );

await interaction.reply({
  components: [container],
  flags: MessageFlags.IsComponentsV2,
});
```

---

## Handling the Selection

```js
client.on('interactionCreate', async interaction => {
  if (!interaction.isStringSelectMenu()) return;

  if (interaction.customId === 'category_select') {
    const chosen = interaction.values[0]; // e.g. 'billing'
    await interaction.reply({ content: `You selected: **${chosen}**`, ephemeral: true });
  }
});
```

---

## Multiple Selections Allowed

```js
new StringSelectMenuBuilder()
  .setCustomId('role_picker')
  .setPlaceholder('Pick up to 3 roles')
  .setMinValues(1)
  .setMaxValues(3)
  .addOptions(
    new StringSelectMenuOptionBuilder().setLabel('Announcements').setValue('ann'),
    new StringSelectMenuOptionBuilder().setLabel('Updates').setValue('upd'),
    new StringSelectMenuOptionBuilder().setLabel('Events').setValue('evt')
  )
```

`interaction.values` will be an array of all selected values.

---

## Option with Emoji and Default

```js
new StringSelectMenuOptionBuilder()
  .setLabel('Billing')
  .setValue('billing')
  .setDescription('Payment and subscription issues')
  .setEmoji('💳')
  .setDefault(false)
```

---

## User Select Menu

Lets the user pick someone from the server — no options to define:

```js
const { UserSelectMenuBuilder } = require('discord.js');

const select = new UserSelectMenuBuilder()
  .setCustomId('user_picker')
  .setPlaceholder('Pick a user...');

// Handle it:
client.on('interactionCreate', async interaction => {
  if (!interaction.isUserSelectMenu()) return;
  const pickedUser = interaction.users.first();
  await interaction.reply({ content: `You picked ${pickedUser}`, ephemeral: true });
});
```

---

## Role Select Menu

```js
const { RoleSelectMenuBuilder } = require('discord.js');

const select = new RoleSelectMenuBuilder()
  .setCustomId('role_picker')
  .setPlaceholder('Pick a role...');

// Handle it:
client.on('interactionCreate', async interaction => {
  if (!interaction.isRoleSelectMenu()) return;
  const pickedRole = interaction.roles.first();
  await interaction.reply({ content: `You picked ${pickedRole}`, ephemeral: true });
});
```

---

## Channel Select Menu

```js
const { ChannelSelectMenuBuilder, ChannelType } = require('discord.js');

const select = new ChannelSelectMenuBuilder()
  .setCustomId('channel_picker')
  .setPlaceholder('Pick a channel...')
  .addChannelTypes(ChannelType.GuildText); // only show text channels
```

---

## Disabled Select Menu

```js
new StringSelectMenuBuilder()
  .setCustomId('disabled_select')
  .setPlaceholder('Not available right now')
  .setDisabled(true)
  .addOptions(
    new StringSelectMenuOptionBuilder().setLabel('Option').setValue('x')
  )
```

---

## Updating the Message After a Selection

```js
client.on('interactionCreate', async interaction => {
  if (!interaction.isStringSelectMenu() || interaction.customId !== 'category_select') return;

  const category = interaction.values[0];

  const updated = new ContainerBuilder()
    .addTextDisplayComponents(
      new TextDisplayBuilder().setContent(`**Category:** ${category}\n\nA staff member will be with you shortly.`)
    );

  await interaction.update({
    components: [updated],
    flags: MessageFlags.IsComponentsV2,
  });
});
```

---

## Notes

- Only **1 select menu** per `ActionRow`
- Up to **25 options** in a `StringSelectMenuBuilder`
- `custom_id` can be up to **100 characters**
- Use `interaction.values` to get selected strings; use `interaction.users`, `interaction.roles`, or `interaction.channels` for entity selects
