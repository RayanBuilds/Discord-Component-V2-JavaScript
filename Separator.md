# Separator

`SeparatorBuilder` adds a horizontal dividing line or spacing between components. Great for splitting a title from body content or separating visual sections.

---

## Basic Usage

```js
const { SeparatorBuilder, SeparatorSpacingSize } = require('discord.js');

const separator = new SeparatorBuilder()
  .setDivider(true)
  .setSpacing(SeparatorSpacingSize.Small);
```

---

## Spacing Variants

```js
new SeparatorBuilder().setSpacing(SeparatorSpacingSize.Small)
new SeparatorBuilder().setSpacing(SeparatorSpacingSize.Large)
```

`Small` adds a tight gap. `Large` adds more breathing room.

---

## Invisible Separator (Spacer Only)

If you want spacing without the visible line:

```js
new SeparatorBuilder().setDivider(false).setSpacing(SeparatorSpacingSize.Small)
```

This adds whitespace between components without drawing a line.

---

## Inside a Container

```js
const { ContainerBuilder, TextDisplayBuilder, SeparatorBuilder, SeparatorSpacingSize } = require('discord.js');

const container = new ContainerBuilder()
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent('### Ticket Closed')
  )
  .addSeparatorComponents(
    new SeparatorBuilder().setDivider(true).setSpacing(SeparatorSpacingSize.Small)
  )
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent(`Closed by <@${user.id}>`)
  )
  .addSeparatorComponents(
    new SeparatorBuilder().setDivider(true).setSpacing(SeparatorSpacingSize.Small)
  )
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent('A transcript has been sent to your DMs.')
  );
```

---

## Notes

- Separators are purely visual — they carry no data
- You can use as many separators as needed inside a container
- Default `setDivider(true)` shows a visible line; `false` is just spacing
