---
title: Logseq | EDN_Settings
description: "Settings for Logseq's config.edn: a macro that draws a progress bar, a slash command that inserts ≠, and line wrapping in code blocks. Sources are listed at the end."
tags:
  - logseq
date: 2022-04-08
lastMod: 2026-10-05
---

Settings that go in Logseq's `config.edn`. Each section says what the setting does, then shows it;
sources are collected at the bottom. CSS tweaks live in
[Logseq | CSS_Theme]({{< relref "CSS_Theme.md" >}}).

## Macros

### Progress bar

**Goal.** Show a small progress bar, such as 47 of 195, inside a block.

**How.** Add a `progress` entry to the `:macros` map. The macro renders a Hiccup `progress`
element followed by the numbers:[^progress]

```edn
{"progress" "[:span
			 [:progress {:value $1 :max $2
              :style {:width 200
              :margin-right 4}}]
              [:small \"$1/$2\"]]"}
```

Call it with the current value and the maximum, for example `{{progress 47,195}}`. The bar's
colours and rounded corners come from CSS; the matching style is the progress bar section of
[Logseq | CSS_Theme]({{< relref "CSS_Theme.md" >}}).

## Editor

### Insert `≠` from the slash menu

**Goal.** Type `≠` without hunting for the character.

**How.** `:commands` adds custom entries to the slash menu. This pair adds a `/!=` command that
inserts `≠`:[^neq]

```edn
:commands
[["!=" "≠"]]
```

### Wrap long lines in code blocks

**Goal.** Long lines in a code block wrap instead of scrolling sideways.

**How.** Pass `lineWrapping` through to the CodeMirror editor that Logseq uses for code
blocks:[^wrap]

```edn
:editor/extra-codemirror-options {:lineWrapping true}
```

[^progress]: [pengx17 on X: To render a custom progress bar in #Logseq, define a macro in config.edn](https://twitter.com/pengx17/status/1502293155974025218)
[^neq]: [Logseq Discord #themes](https://discord.com/channels/725182569297215569/752845138148982877/951915033884000266)
[^wrap]: [Logseq Discord #general](https://discord.com/channels/725182569297215569/725182570131751005/963372513348423690)
