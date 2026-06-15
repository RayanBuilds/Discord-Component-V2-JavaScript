# File Component

`FileBuilder` lets you display a file attachment **inline** inside a CV2 layout — transcripts, logs, exports, anything.

---

## Basic Usage

You need two things:
1. An `AttachmentBuilder` (the actual file to upload)
2. A `FileBuilder` referencing it via `attachment://filename`

```js
const { FileBuilder, AttachmentBuilder, ContainerBuilder, TextDisplayBuilder, MessageFlags } = require('discord.js');

const file = new AttachmentBuilder('./transcript.txt').setName('transcript.txt');
const fileComponent = new FileBuilder().setURL('attachment://transcript.txt');

await channel.send({
  components: [fileComponent],
  files: [file],
  flags: MessageFlags.IsComponentsV2,
});
```

---

## Inside a Container

```js
const container = new ContainerBuilder()
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent('### Ticket Transcript')
  )
  .addSeparatorComponents(
    new SeparatorBuilder().setDivider(true).setSpacing(SeparatorSpacingSize.Small)
  )
  .addTextDisplayComponents(
    new TextDisplayBuilder().setContent(`Ticket closed by <@${user.id}>`)
  )
  .addFileComponents(
    new FileBuilder().setURL('attachment://ticket-0042-transcript.txt')
  );

const attachment = new AttachmentBuilder('./ticket-0042-transcript.txt');

await channel.send({
  components: [container],
  files: [attachment],
  flags: MessageFlags.IsComponentsV2,
});
```

---

## From a Buffer

```js
const { Readable } = require('stream');

const content = 'Hello, this is the file content.';
const stream = Readable.from([content]);
const file = new AttachmentBuilder(stream).setName('output.txt');
const fileComponent = new FileBuilder().setURL('attachment://output.txt');
```

---

## Sending to a User's DMs

```js
await user.send({
  components: [fileComponent],
  files: [file],
  flags: MessageFlags.IsComponentsV2,
});
```

---

## Spoiler File

```js
new FileBuilder().setURL('attachment://secret.txt').setSpoiler(true)
```

---

## Notes

- The filename in `attachment://` must **exactly match** the name set on `AttachmentBuilder` — it's case-sensitive
- Only **one** `FileBuilder` per message (Discord limitation)
- Always call `.setName()` on the `AttachmentBuilder` so the filename is explicit
