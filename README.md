# anmail

A thin wrapper around [himalaya](https://github.com/pimalaya/himalaya).

## What it is

anmail is a thin wrapper around himalaya，which aims to fill the gaps in `himalaya`'s interaction.

It doesn't implement IMAP/SMTP, nor does it manage anthentication or passwords.

All receiving, sending, authentication, and synchronization are handled by `himalaya`.

## Why

`himalaya`'s `message compose` doesn't auto open an editor,
and `message send` requires the 'stdin' to be a complete MIME message.

Composing mail directly with `himalaya` means either assembling headers

in the terminal, or relying on a Vim/Neovim plugin -- and

the plugin will hangs when it calls `sudo` from a child process without a TTY.

`anmail` decouples these two step:

1. The editor runs in the foreground of the terminal

2. After you save&quit, the same shell process pipe the content to `himalaya`.

sudo has a TTY now, and the editor has interaction, each does it job.

This is where anmail starts. I can use `himalaya` in my way now.

## Requirements
- [himalaya](https://github.com/pimalaya/himalaya) —— **IMAP/SMTP must be configured**
- An `$EDITOR` ( defaults to `nvim`)

## Install

put `anmail` in `$PATH`(e.g. `~/.local/bin/`) , and `chmod +x` the guy:

```bash
git clone https://github.com/Annaler/anmail && cd anmail
cp anmail ~/.local/bin/anmail
chmod +x ~/.local/bin/anmail
```

## Configuration

Set sender in `~/.bashrc` or `~/.zshrc`:

```bash
export MAIL_FROM='"Display Name" <email@example.com>'
```

## Usage

```bash
send <To> [Subject]
```
Wrote body in editor, header from your command, 

compose them to a mail and give it to `himalaya` by pipe. Then, `himalaya` "send it home".

## Roadmap
- [x] `send <To> [Subject]` -- compose and send a mail
- [ ] `list`
- [ ] `read <ID>`
- [ ] `reply <ID>`
- [ ] ...


