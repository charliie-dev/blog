# Logseq | Tips

Small Logseq tips collected from the Logseq Discord. Each section says what the tip does, then how;
sources are collected at the bottom.

## Tasks and pages

### Task states at a glance

The diagram below shows every task state Logseq implements and how they move into each
other:[^task-states]

![Logseq implemented task states](https://raw.githubusercontent.com/charleschiu2012/image-hosting/main/img/Logseq_implemented_task_states.png)

### Show a project's status with an `icon::` property

**Goal.** See at a glance whether a project is moving, cancelled or done.

**How.** Give each project page an `icon::` page property with an emoji, such as a stop sign or a
green traffic light. The icon shows wherever the page or a link to it appears, and it can still be
queried by matching the emoji, so it gives both the visual cue and an easy query.[^icon] The query
itself is in [Logseq | Queries: Pages with an `icon::` property]({{< relref "Queries.md#pages-with-an-icon-property" >}}).

### Query a page inside a namespace

In an advanced query, replace the slash in a namespaced page name with an underscore: the page
`projects/2022` is matched as `"projects_2022"`.

### Table of contents for a namespace

Put `{{namespace keyword}}` in the Contents sidebar to get a table of contents that updates by
itself. For example, `{{namespace Logseq}}` lists every page under `Logseq/`.[^namespace]

## iOS shortcuts

### Save a Safari selection as a quote

An iOS shortcut sends the text selected in Safari to Logseq as a quote, together with a link back
to the page.[^safari][^safari-shortcut]

### Dictate a voice note into today's journal

Built on @sawhneys's shortcut, this one adds dictated text to today's journal as a note tagged
`#voicenote`.[^voice][^voice-shortcut]

## Writing

### Hide block properties until hover

**Goal.** Keep properties out of the way but one hover away.

**How.** Hide the properties block and show a small indicator in its place; hovering reveals the
properties and hides the indicator. The indicator can be anything, but it must exist, or there is
nothing to hover over:[^hide-props]

```css
.content .block-properties {
  visibility: hidden;
  position: relative;
}

.content .block-properties:hover {
  visibility: visible;
}

.content .block-properties::after {
  visibility: visible;
  position: absolute;
  top: 0;
  left: 0;
  content: "👻";
}

.content .block-properties:hover::after {
  content: "";
}
```

### Write a dollar sign inside LaTeX

**Problem.** `$` ends inline LaTeX, so `$6*8=$48` cannot be written directly.

**Fix.** Write `\char36` for each dollar sign inside `$...$`:[^dollar]
`$\char36 6 \times 8 = \char36 48$` renders as $\char36 6 \times 8 = \char36 48$.

### Equations where the editor has no LaTeX

**Goal.** Show an equation in Markdown that the editor cannot render as LaTeX.

**How.** Embed the equation as an image rendered by CodeCogs, with the LaTeX in the URL. The tip
comes from the Dengron Discord server:[^dengron][^codecogs]

![pi](<https://latex.codecogs.com/png.latex?\frac{1}{\pi}=\frac{2\sqrt{2}}{9801}\sum_{k=0}^\infty\frac{(4k)!(1103%2B26390k)}{(k!)^4396^{4k}}>)

### Insert an audio player

Use the image syntax with an audio file and Logseq shows a player:[^audio]
`![](assets/audio.mp3)`.

## Integrations

### Run Logseq in Docker

Logseq's repository documents how to run the web app in a Docker container.[^docker][^docker-guide]

### Embed Jupyter through JupyterLite

**Goal.** Run a Jupyter notebook inside a Logseq page.

**How.** JupyterLite runs Jupyter entirely in the browser through WebAssembly, so it can be
embedded with an iframe:[^jupyter][^jupyterlite]

```html
<iframe
  src="https://jupyterlite.github.io/demo/lab/"
  frameborder="0"
  scrolling="YES"
  allowtransparency="true"
  sandbox="allow-same-origin allow-scripts allow-popups allow-popups-to-escape-sandbox"
  style="width: 100%; height: 800px"
></iframe>
```

### The URL protocol

An overview and a breakdown of Logseq's `logseq://` URL protocol:[^url]

![Logseq URL protocol overview](https://raw.githubusercontent.com/charleschiugit/image-hosting/main/img/Logseq%20URL%20Protocol%20overview.png)

![Logseq URL protocol breakdown](https://raw.githubusercontent.com/charleschiugit/image-hosting/main/img/Logseq%20URL%20Protocol%20breakdown.png)

## Trivia

### How to pronounce Logseq

![How to pronounce Logseq](https://raw.githubusercontent.com/charleschiugit/image-hosting/main/img/HowToPronounceLogseq.png)

A diagram shared in #look-what-i-built.[^pronounce]

### Vault survival kit

![PKM vault survival kit](https://raw.githubusercontent.com/charleschiu2012/image-hosting/main/img/PKM_Vault_survival_kit.png)

Also from #look-what-i-built.[^vault]

### Why the logo works

A comment in #design on the current [Logseq logo design](https://www.figma.com/community/file/933752127976667301):[^logo]

> imo the best iteration on the logo is still Logseq Logo design. it's minimalistic so it scales
> well, it works in color and b&w, it works great as a static logo and animated logo, the dots are
> reminiscent of nodes, the shape is the L of logseq, the moving dots hint at the connection
> between nodes and the ever-evolving nature of a graph, and it still inherits traits from the
> previous logos, while setting a different mood from competitors (roam, obsidian, craft, clover,
> …)
>
> imo, the creator really did a great job on this (open the figma to see preliminary steps). The
> only thing that may be missing is the 'playful' or 'joyful' / kids-friendly aspect that seemed
> important for @tienson but that could be addressed with hints of color (on one or multiple dots)

[^task-states]: [Logseq Discord #general](https://discord.com/channels/725182569297215569/725182570131751005/952564162792402976)
[^icon]: [Logseq Discord #workflows](https://discord.com/channels/725182569297215569/766475028978991104/961627375370661918)
[^namespace]: [Logseq Discord #tips](https://discord.com/channels/725182569297215569/740582434961358848/963821349917319219)
[^safari]: [Logseq Discord #ios-app](https://discord.com/channels/725182569297215569/924907384730689566/955548362139119706)
[^safari-shortcut]: [iCloud Shortcut: Add to Logseq](https://www.icloud.com/shortcuts/a5fb574078e2494da34fd4c468d9c1ef)
[^voice]: [Logseq Discord #ios-app](https://discord.com/channels/725182569297215569/924907384730689566/956086408819392523)
[^voice-shortcut]: [iCloud Shortcut: Logseq Voice Note Today](https://www.icloud.com/shortcuts/da2d44bf4e9e4f17a92bec5472cc8384)
[^hide-props]: [Logseq Discord #themes](https://discord.com/channels/725182569297215569/752845138148982877/906275176742801410)
[^dollar]: [Logseq Discord #general](https://discord.com/channels/725182569297215569/725182570131751005/963510550124437504)
[^dengron]: [Discord Link](https://discord.com/channels/717965437182410783/904891933284007966/956934748721270784)
[^codecogs]: [CodeCogs Equation Editor: Quickstart](https://editor.codecogs.com/docs)
[^audio]: [Logseq Discord #tips](https://discord.com/channels/725182569297215569/740582434961358848/967518539785310228)
[^docker]: [Logseq Discord #general](https://discord.com/channels/725182569297215569/725182570131751005/956622800716705832)
[^docker-guide]: [logseq/logseq: docs/docker-web-app-guide.md](https://github.com/logseq/logseq/blob/master/docs/docker-web-app-guide.md)
[^jupyter]: [Logseq Discord #off-topic](https://discord.com/channels/725182569297215569/736514221499744287/967240289888641158)
[^jupyterlite]: [JupyterLite: Jupyter ❤️ WebAssembly ❤️ Python](https://blog.jupyter.org/jupyterlite-jupyter-%EF%B8%8F-webassembly-%EF%B8%8F-python-f6e2e41ab3fa)
[^url]: [Logseq Discord #look-what-i-built](https://discord.com/channels/725182569297215569/756886540038438992/965024044183339088)
[^pronounce]: [Logseq Discord #look-what-i-built](https://discord.com/channels/725182569297215569/756886540038438992/957664768553001050)
[^vault]: [Logseq Discord #look-what-i-built](https://discord.com/channels/725182569297215569/756886540038438992/952605818791030866)
[^logo]: [Logseq Discord #design](https://discord.com/channels/725182569297215569/775936939638652948/934860582799147009)

