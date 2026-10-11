# anmail

A thin wrapper around [himalaya](https://github.com/pimalaya/himalaya).

**Version**: v0.3.0 (2026-10-11)

## What it is

anmail is a thin wrapper around himalaya that aims to fill the gaps in `himalaya`'s interaction.

It doesn't implement IMAP/SMTP, nor does it manage authentication or passwords.

All receiving, sending, authentication, and synchronization are handled by `himalaya`.

## Why

`himalaya`'s `message compose` doesn't auto open an editor,
and `message send` requires the `stdin` to be a complete MIME message.

Composing mail directly with `himalaya` means either assembling headers
in the terminal, or relying on a Vim/Neovim plugin — and
the plugin will hang when it calls `sudo` from a child process without a TTY.

`anmail` decouples these two steps:

1. The editor runs in the foreground of the terminal

2. After you save & quit, the same shell process pipes the content to `himalaya`.

Now sudo has a TTY and the editor stays interactive — each does its job.

This is where anmail starts. I can use `himalaya` in my own way now.

## Requirements

- [himalaya](https://github.com/pimalaya/himalaya) — **IMAP/SMTP must be configured**
- An `$EDITOR` (defaults to `nvim`)

## Install

Put `anmail` in `$PATH` (e.g. `~/.local/bin/`), and `chmod +x` the guy:

```bash
git clone https://github.com/Annaler/anmail && cd anmail
cp anmail ~/.local/bin/anmail
chmod +x ~/.local/bin/anmail
```

## Configuration

Account live in `~/.config/anmail/accounts.config`

```
default="Your Name" <you@example.com>
work="Work Name" <work@company.com>
personal=me@outlook.com
```

Manage them with `anmail set`:

```bash
anmail set work '"Work Name" <work@company.com>'
anmail set -i
anmail set (list)
```

The account name before '=' is what u pass to `-a / --account`
Override the file path with `ANMAIL_CONFIG`

## Usage

```bash
anmail send <To> [Subject]
```
Write the body in your editor; the header comes from your command.
`anmail` composes them into a mail and pipes it to `himalaya message send`.
Then, `himalaya` "send them home".

```bash
anmail list [options]...
```
Passes the command straight through to `himalaya envelope list`.
Simple as this — ashit.

```bash
anmail read <ID> [options]
```
Just like the previous one, but `himalaya message read`.

```bash
anmail set list
anmail set <name> <from>
anmail set -i [name] [from]
```
List, set, or interactively edit accounts.


## Roadmap

- [x] `send <To> [Subject]` — compose and send a mail
- [x] `list` — just pass "words" to himalaya (1st working ver.)
- [x] `read <ID>` — same as the previous one
- [x] `set` (list) - set/list account config
- [ ] `reply <ID>` — 考虑加入自动对原文保留
- [ ] ...

## Versioning

- `0.x.y` — `x` increments on new features, `y` on fixes & optimizations.
- `1.0.0` — if it is, it's the almost complete primary realization of basic features.
