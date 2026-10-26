# pi-intercom

Messaging between Pi sessions on the same machine.

The `intercom` tool lets a session:

- list the other sessions, with their name, directory, model, and whether they're busy
- send a message to a session, which starts a turn there
- ask a session a question and get the reply back without blocking; questions time out after 10 minutes
- read another session's recent conversation from its saved session file, without interrupting it
- publish a label so other sessions can find it

## Install

```bash
pi install npm:@ryanmazzolini/pi-intercom
```

Needs Node.js 24 or newer on your `PATH`. Runs on macOS and Linux. Tested with Pi 0.85.1 and 1.0.4.

Pi won't start if another extension also adds a tool named `intercom`. Remove or disable it first.

## How it works

Sessions connect to a broker over a Unix socket at `~/.pi/agent/intercom/broker.sock`. The first session that needs it starts it in the background with `node`. It exits about 5 seconds after the last session disconnects.

Messages stay on your machine until a session reads them. From then on they're part of that session's context and go to its model provider like anything else.

Limits:

- Labels only help sessions find each other. They grant nothing, several sessions can share one, and a reload, compaction, tree navigation, or reconnect clears them.
- Reading a session shows user and assistant messages and whether each tool call succeeded, not thinking, images, or tool output. Only sessions saved to a file can be read.
- Output to the model is capped at 48 KiB and marked where cut. Replies to questions get 40 KiB, so the question preview fits alongside them.

## Credit

Based on Nico Bailon's [`pi-intercom`](https://www.npmjs.com/package/pi-intercom/v/0.6.0). See [THIRD_PARTY_NOTICES.md](./THIRD_PARTY_NOTICES.md).

## License

MIT. Extracted from [`pi-skills`](https://github.com/ryanmazzolini/pi-skills), where its earlier history lives.
