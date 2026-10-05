---
title: Logseq | EDN_Settings
description: "config.edn tweaks for Logseq: a custom progress-bar macro with its CSS, rendering != as ≠, and changing the foreground and background colours of highlighted text. Each snippet links back to the Discord or Twitter thread it came from."
tags:
  - logseq
date: 2022-04-08
lastMod: 2022-04-12
---

Render a custom progress bar in Logseq, define a macro in `config.edn`:

```edn
{"progress" "[:span
			 [:progress {:value $1 :max $2
              :style {:width 200
              :margin-right 4}}]
              [:small \"$1/$2\"]]"}
```

- For example Write `{{progress 47,195}}`.

- [pengx17 on X: To render a custom progress bar in #Logseq, define a macro in config.edn](https://twitter.com/pengx17/status/1502293155974025218)

- the style of the process bar in css:

  - Progress par style in `custom.css`:

---

Make `!=` looks like `≠`:

- [Logseq Discord #themes](https://discord.com/channels/725182569297215569/752845138148982877/951915033884000266)

```edn
:commands
[
  [“!=“ “≠”]
]
```

---

A funny experiment to turn blocks / children-blocks into resizable blocks that behave like flex-boxes

- [Logseq Discord #themes](https://discord.com/channels/725182569297215569/752845138148982877/951186890328002570)

```edn
/* ls-blocks : resizeable children-blocks + depth levels */
.block-children .ls-block {
                           display: inline-block;
                           margin: 6px;
                           background-color: rgba(50, 55, 60, 0.5);
                           box-shadow: 1px 1px 5px rgba(0, 0, 0, 0.46);
                           padding: 2px 6px 8px 6px;
                           resize: both;
                           overflow: auto;
                           min-width: 160px;
                           min-height: 30px;
                           width: fit-content;
                           height: fit-content;
                           vertical-align: top;
                           }
```

---

Change my highlighted text's fore and back colors

- [Logseq Discord #themes](https://discord.com/channels/725182569297215569/752845138148982877/950759618638917652)

```css
mark {
  background: #fef3ac;
  color: #262626;
  padding: 2px 4px;
  border-radius: 3px;
}
```

---

line wrap codes

- [Logseq Discord #general](https://discord.com/channels/725182569297215569/725182570131751005/963372513348423690)

```edn
:editor/extra-codemirror-options {:lineWrapping true}
```

---
