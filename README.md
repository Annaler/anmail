# anmail

A thin wrapper around [himalaya](https://github.com/pimalaya/himalaya).

**Version**: v0.3.3 (2026-10-12)

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
- An `VISUAL` [defaults to `$EDITOR` (defaults to `nvim`)]

ps: `VISUAL`/`EDITOR` must be a single executable path.
Values with arguments (e.g. `code --wait`) are not supported.
If you really want, may tryna write a wrapper script?

## Install

Put `anmail` in `$PATH` (e.g. `~/.local/bin/`), and `chmod +x` the guy:

```bash
git clone https://github.com/Annaler/anmail && cd anmail
cp anmail ~/.local/bin/anmail
chmod +x ~/.local/bin/anmail
```

Or... u can use soft link, belike:

```bash
git clone https://github.com/Annaler/anmail && cd anmail
chmod +x anmail
ln -s "$(pwd)/anmail" ~/.local/bin/ama
#Us any name u like here  ^^^
```

By the way, just by `git pull`, u can update the script.

## Configuration

Accounts live in `ANMAIL_CONFIG` (default to `~/.config/anmail/accounts.config`):
```
[using]
current=me
[accounts]
#tag=from
me="Your Name" <you@example.com>
work="Work Name" <work@company.com>
personal=private_mail@outlook.com
```

Manage them with `anmail set`:

```bash
anmail set work '"Work Name" <work@company.com>'
anmail set -i
anmail set list
```

The account tag before '=' is what u pass to `-a / --account`,
so the <tag> must be the same as what u configurated in himalaya's config
       ^   ^ => [accounts.<tag>] <- ~/.config/himalaya/config.toml
                          ^   ^

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
anmail set <tag> <from>
anmail set -i [tag] [from]
```
List, set, or interactively edit accounts.

```bash
anmail use
anmail use <tag>
```
Show the account you're using,
or switch to account `<tag>`.

## Roadmap

- [x] `send <To> [Subject]` — compose and send a mail
- [x] `list` — just pass "words" to himalaya (1st working ver.)
- [x] `read <ID>` — same as the previous one
- [x] `set` (list) — set/list account config
- [x] `use [account]` — switch account to use
- [ ] `reply <ID>` — 考虑加入自动对原文保留
- [ ] ...

## Versioning

- `0.x.y` — `x` increments on new features, `y` on fixes & optimizations.
- `1.0.0` — if it is, it's the almost complete primary realization of basic features.
ps: before `v1.0.0` is released, breaking changes may occur even if there are not major version updates.
