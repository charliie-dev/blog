---
title: Vim | Cheatsheet
description: "My Vim cheatsheet, built around Vim's grammar of command plus motion or text object: moving within a line and through a file, editing with change, the dot command, macros and registers, search and replace with Vim's regex rules, folds, buffers, windows and tabs, and running shell commands. Sources are listed at the end."
tags:
  - vim
  - devlog
date: 2022-12-04
lastMod: 2026-10-06T00:00:00+08:00
---

The Vim keys I actually use, grouped by task. Most of it comes from ThePrimeagen's videos and the
Vim Tips Wiki; sources are collected at the bottom.

## Before you start

- **Raise the keyboard repeat rate.** Holding `j` or `w` is far less painful when keys repeat
  quickly. On Windows this lives in the keyboard settings.[^repeat]
- **Read `:help`.** `:h` opens the manual, and `:h <topic>` jumps to a topic, such as `:h g` for
  every command that starts with `g`.

## The grammar: command, then motion or text object

Vim edits are sentences. A **command** says what to do, and a **motion** or **text object** says
what to do it to: `{command}{motion or text object}`. A count in front repeats it, so `3dw`
deletes three words.

| Commands | Does                                   |
| -------- | -------------------------------------- |
| `d`      | Delete (also cuts into a register)     |
| `c`      | Change: delete, then enter insert mode |
| `y`      | Yank (copy)                            |
| `v`      | Select visually                        |

| Motions and modifiers | Means                                   |
| --------------------- | --------------------------------------- |
| `i`                   | Inside the object                       |
| `a`                   | Around the object, including delimiters |
| `t{char}`             | Until the character                     |
| `f{char}` / `F{char}` | Find the character forward / backward   |

| Text objects        | Means                                |
| ------------------- | ------------------------------------ |
| `w` / `W`           | Word / WORD (anything up to a space) |
| `s`                 | Sentence                             |
| `p`                 | Paragraph                            |
| `t`                 | Tag, in XML and HTML                 |
| `(` `[` `{` `"` `'` | The pair of brackets or quotes       |

Put together:

| Keys          | Does                                                         |
| ------------- | ------------------------------------------------------------ |
| `diw`         | Delete the word under the cursor                             |
| `caw`         | Change the word and the space around it                      |
| `ciw`         | Change the word under the cursor                             |
| `yi)`         | Yank everything inside the parentheses                       |
| `va'`         | Select a quoted string, quotes included                      |
| `di]` / `da[` | Delete inside the brackets / including the brackets          |
| `dt-`         | Delete up to the next `-`; `vt-` and `yt-` work the same way |
| `viW`         | Select everything up to the next space                       |

`vi(` and its friends also work when the cursor is not yet inside the brackets: Vim looks ahead
on the line for the next pair.[^blazing]

## Moving within a line

| Keys            | Does                                                        |
| --------------- | ----------------------------------------------------------- |
| `w` / `W`       | Start of the next word / WORD                               |
| `e` / `E`       | End of the word / WORD                                      |
| `b` / `B`       | Start of the previous word / WORD                           |
| `f{c}` / `F{c}` | Onto the next / previous `{c}` on the line                  |
| `t{c}` / `T{c}` | Just before the next / just after the previous `{c}`        |
| `;` / `,`       | Repeat the last `f`, `t`, `F` or `T` / repeat it in reverse |
| `0`             | Start of the line                                           |
| `^`             | First non-blank character                                   |
| `$`             | End of the line                                             |
| `*` / `#`       | Next / previous occurrence of the word under the cursor     |

## Moving through a file

| Keys                | Does                                                |
| ------------------- | --------------------------------------------------- |
| `gg` / `G`          | Top / bottom of the file                            |
| `:42`               | Line 42                                             |
| `Ctrl-u` / `Ctrl-d` | Half a page up / down                               |
| `{` / `}`           | Previous / next blank line                          |
| `%`                 | The matching bracket                                |
| `[m`                | Start of the current method                         |
| `[[`                | Start of the previous section, often a function     |
| `Ctrl-o`            | Back to where the last jump started (the jump list) |
| `Ctrl-t`            | Back from a jump to a definition (the tag stack)    |

## Editing

### Inserting and changing

| Keys                | Does                                                                 |
| ------------------- | -------------------------------------------------------------------- |
| `I` / `A`           | Insert at the start / end of the line                                |
| `o` / `O`           | Open a new line below / above and insert                             |
| `x`                 | Delete the character under the cursor                                |
| `s`                 | Delete the character and insert                                      |
| `S`                 | Delete the whole line and insert                                     |
| `D` / `C`           | Delete / change from the cursor to the end of the line               |
| `cw`                | Change from the cursor to the end of the word                        |
| `Ctrl-a` / `Ctrl-x` | Increment / decrement the number under the cursor, or in a selection |

### Copying, deleting and pasting

| Keys          | Does                                                  |
| ------------- | ----------------------------------------------------- |
| `yw`          | Yank from the cursor to the start of the next word    |
| `yiw` / `yaw` | Yank the word / the word and its surrounding space    |
| `Y` or `y$`   | Yank to the end of the line                           |
| `Vy` / `Vd`   | Yank / delete the whole line                          |
| `dw` / `diw`  | Delete to the start of the next word / the whole word |

ThePrimeagen prefers `Vy` and `Vd` to `yy` and `dd`: two different keys are faster to press than
the same key twice.[^blazing]

Pasting over a selection normally replaces the register with the text you pasted over. This
mapping deletes the selection into the black-hole register first, so the same text can be pasted
again and again:[^blazing]

```vim
xnoremap <leader>p "_dP
```

### Repeating

- **The dot command.** `.` repeats the last change. It pairs best with `c`: change one word, move
  to the next, press `.`.
- **Macros.** `q{register}` starts recording into a register, `q` stops, and `@{register}` plays
  it back.

## Search and replace

`:s` works on the current line; a range in front widens it:[^replace]

| Command                | Replaces `foo` with `bar`                    |
| ---------------------- | -------------------------------------------- |
| `:s/foo/bar/g`         | On the current line                          |
| `:%s/foo/bar/g`        | In the whole file                            |
| `:5,12s/foo/bar/g`     | On lines 5 to 12                             |
| `:.,$s/foo/bar/g`      | From the current line to the end of the file |
| `:.,+2s/foo/bar/g`     | On the current line and the next two         |
| `:'<,'>s/\%Vfoo/bar/g` | Only inside the visual selection             |
| `:g/^baz/s/foo/bar/g`  | On every line that starts with `baz`         |

Pressing `:` with a selection active fills in the `'<,'>` range for you.

### Patterns

- `.`, `*`, `\`, `[`, `^` and `$` are special as they are. `+`, `?`, `|`, `&`, `{`, `(` and `)`
  need a backslash to become special.
- `\/` matches a slash, `\t` a tab, `\s` a space or tab, `\n` a newline and `\r` a carriage
  return.
- `[1a-c]` matches one of the listed characters; `[^1a-c]` matches anything else.
- `\{2}` repeats the previous item, so `/foo.\{2}` matches `foo` and the next two characters.
- `\(foo\)` captures a group for a backreference.

### Replacements

- `\r` inserts a newline; `\n` inserts a null byte.
- `&` or `\0` inserts the whole match, and `\&` a literal ampersand.
- `\1`, `\2` and so on insert the captured groups.
- `\zs` and `\ze` mark where the match really starts and ends, so the surrounding text only has to
  match, not be retyped:

```vim
:s/Copyright \zs2007\ze All Rights Reserved/2008/
```

## Folds

Set how folds are made with `:set foldmethod=...`: `manual`, `marker` (sections wrapped in `{{{`
and `}}}`), `syntax` (by the language's syntax) or `diff` (unchanged lines, set automatically in
diff mode).[^folding]

| Keys        | Does                                       |
| ----------- | ------------------------------------------ |
| `zf`        | Create a fold from a motion or a selection |
| `zo` / `zc` | Open / close the fold                      |
| `za`        | Toggle the fold                            |
| `zR` / `zM` | Open / close every fold                    |
| `zd` / `zD` | Delete the fold / delete it recursively    |

## Buffers, windows and tabs

A **buffer** is a file's text in memory, a **window** is a view onto a buffer, and a **tab** is a
collection of windows.

| Keys                    | Does                                        |
| ----------------------- | ------------------------------------------- |
| `:e <file>`             | Open a file                                 |
| `:ls`                   | List the buffers                            |
| `:b 3` / `:b name`      | Go to buffer 3 / the buffer matching `name` |
| `:bn` / `:bp`           | Next / previous buffer                      |
| `:bf` / `:bl`           | First / last buffer                         |
| `Ctrl-^`                | Back to the previous buffer                 |
| `:bdelete`              | Close the buffer                            |
| `:tab ball`             | Open every buffer in its own tab            |
| `Ctrl-w v` / `Ctrl-w s` | Split the window vertically / horizontally  |
| `Ctrl-w o`              | Close every window but the current one      |

## Shell commands and starting Vim

| Command                 | Does                                               |
| ----------------------- | -------------------------------------------------- |
| `:!cmd`                 | Run a shell command                                |
| `:.!cmd`                | Replace the current line with the command's output |
| `cmd \| vim -`          | Open a command's output in Vim                     |
| `vim +42 file`          | Open a file at line 42                             |
| `vim +/pattern file`    | Open a file at the first match                     |
| `vim -c "cmd" file`     | Run an Ex command after opening the file           |
| `vim -d file1 file2`    | Compare two files                                  |
| `:vert diffsplit file2` | Compare the open file with another one             |
| `:diffoff`              | Leave diff mode                                    |
| `:so %`                 | Source the current file, such as `init.vim`        |
| `:digraphs`             | List the special characters you can type           |

## Finding keymaps

| Command                  | Does                                      |
| ------------------------ | ----------------------------------------- |
| `:nmap`                  | List every normal-mode mapping            |
| `:nmap <leader>`         | List the mappings that start with leader  |
| `:verbose nmap <leader>` | Also show where each mapping was defined  |
| `:Telescope keymaps`     | Search mappings with the Telescope plugin |

In Neovim, `:lua print(vim.inspect(x))` prints a Lua table, which helps when reading
configuration.

[^repeat]: [Change Keyboard Repeat Delay and Rate in Windows 10](https://winaero.com/change-keyboard-repeat-delay-and-rate-in-windows-10/)

[^blazing]: [ThePrimeagen: BLAZINGLY FAST Vim - Part 1](https://youtu.be/qZO9A5F6BZs)

[^replace]: [Vim Tips Wiki: Search and replace](https://vim.fandom.com/wiki/Search_and_replace)

[^folding]: [Vim Tips Wiki: Folding](https://vim.fandom.com/wiki/Folding)
