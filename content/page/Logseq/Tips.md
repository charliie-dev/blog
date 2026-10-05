---
title: Logseq | Tips
description: "Assorted Logseq tips: the task-state diagram, a vault survival kit, saving a Safari selection as a quote with an iOS shortcut and running Logseq in Docker. Also how to pronounce Logseq, the URL protocol, embedding Jupyter through JupyterLite, writing a literal dollar sign in LaTeX and adding an audio player with image syntax."
tags:
  - logseq
date: 2022-04-08
lastMod: 2022-04-25
---

Illustration of Logseq's implemented task states:

- [Logseq Discord #general](https://discord.com/channels/725182569297215569/725182570131751005/952564162792402976)

![Logseq implemented task states](https://raw.githubusercontent.com/charleschiu2012/image-hosting/main/img/Logseq_implemented_task_states.png)

---

Vault survival kit

- [Logseq Discord #look-what-i-built](https://discord.com/channels/725182569297215569/756886540038438992/952605818791030866)

![PKM vault survival kit](https://raw.githubusercontent.com/charleschiu2012/image-hosting/main/img/PKM_Vault_survival_kit.png)

---

Save a current safari selection to Logseq as a quote and link

- [Logseq Discord #ios-app](https://discord.com/channels/725182569297215569/924907384730689566/955548362139119706)

- [iCloud Shortcut: Add to Logseq](https://www.icloud.com/shortcuts/a5fb574078e2494da34fd4c468d9c1ef)

---

While querying a page that uses namespace, replace the slash with underscore. So if the page name is projects/2022, in advanced query it should be "projects_2022"

---

A new shortcut using @sawhneys shortcut as reference to add dictated text as a note to logseq (today) with a tag \#voicenote

- [Logseq Discord #ios-app](https://discord.com/channels/725182569297215569/924907384730689566/956086408819392523)

- [iCloud Shortcut: Logseq Voice Note Today](https://www.icloud.com/shortcuts/da2d44bf4e9e4f17a92bec5472cc8384)

---

Use Logseq with [[Software/Virtualization/Docker/container]]

- [Logseq Discord #general](https://discord.com/channels/725182569297215569/725182570131751005/956622800716705832)

- [logseq/logseq: docs/docker-web-app-guide.md](https://github.com/logseq/logseq/blob/master/docs/docker-web-app-guide.md)

---

You can write equations in Markdown even if your md editor doesn't support Latex, from [[Dengron]] discord server:

- [Discord Link](https://discord.com/channels/717965437182410783/904891933284007966/956934748721270784)

![pi](<https://latex.codecogs.com/png.latex?\frac{1}{\pi}=\frac{2\sqrt{2}}{9801}\sum_{k=0}^\infty\frac{(4k)!(1103%2B26390k)}{(k!)^4396^{4k}}>)

- ref: [CodeCogs Equation Editor: Quickstart](https://editor.codecogs.com/docs)

---

imo the best iteration on the logo is still
[Logseq Logo design](https://www.figma.com/community/file/933752127976667301)
it's minimalistic so it scales well, it works in color and b&w, it works great as a static logo and animated logo, the dots are reminiscent of nodes, the shape is the L of logseq, the moving dots hint at the connection between nodes and the ever-evolving nature of a graph, and it still inherits traits from the previous logos, while setting a different mood from competitors (roam, obsidian, craft, clover, …)

- [Logseq Discord #design](https://discord.com/channels/725182569297215569/775936939638652948/934860582799147009)

- [Logseq Logo design](https://www.figma.com/community/file/933752127976667301)

- imo, the creator really did a great job on this (open the figma to see preliminary steps). The only thing that may be missing is the 'playful' or 'joyful' / kids-friendly aspect that seemed important for @tienson but that could be addressed with hints of color (on one or multiple dots)

---

This would hide the properties, but shows them on hover. The indicator can be anything, but for hover to work, there has to be something.

- [Logseq Discord #themes](https://discord.com/channels/725182569297215569/752845138148982877/906275176742801410)

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

---

How do you pronounce Logseq?

- [Logseq Discord #look-what-i-built](https://discord.com/channels/725182569297215569/756886540038438992/957664768553001050)

- ![How to pronounce Logseq](https://raw.githubusercontent.com/charleschiugit/image-hosting/main/img/HowToPronounceLogseq.png)

---

Add icon to page property for better project-management and visualization.

- [Logseq Discord #workflows](https://discord.com/channels/725182569297215569/766475028978991104/961627375370661918)

- I'm a big fan of namespaces and in this case, what makes the most sense to me is having a tag.

- but one thought that quickly comes to my mind is the usage of the **icon:: page property**. You can perhaps have like a stop sign or like a green traffic light or something to signify that a project is either in motion or cancelled or has been completed. The benefit of this is that you can **easily query**, but also and arguably more importantly, you can **easily and visually see the status of a Projects** by looking at it or a link that's referring to it

- I'm pretty sure you can still query for then you just have to use emoji in the query and I think that this makes a lot of sense and suit offers the best of both worlds. The easy query from the first and the visual look from the second.

- Query for **icon page property**.

---

\$6\*8=\$48 in LaTeX

- [Logseq Discord #general](https://discord.com/channels/725182569297215569/725182570131751005/963510550124437504)

- replace the `$` with `\char36` in `$...$`

- $\char36 6 \times 8 = \char36 48$

---

Add `{{namespace keyword}}` in the Contents section to get automatic Table of Contents.

- [Logseq Discord #tips](https://discord.com/channels/725182569297215569/740582434961358848/963821349917319219)

- {{namespace Logseq}}

---

Logseq URL Protocol overview.

- [Logseq Discord #look-what-i-built](https://discord.com/channels/725182569297215569/756886540038438992/965024044183339088)

- demo: #.v-gallery-col2

- ![Logseq URL protocol overview](https://raw.githubusercontent.com/charleschiugit/image-hosting/main/img/Logseq%20URL%20Protocol%20overview.png)

- ![Logseq URL protocol breakdown](https://raw.githubusercontent.com/charleschiugit/image-hosting/main/img/Logseq%20URL%20Protocol%20breakdown.png)

---

Embed Jupyter that are running in JupyterLite. #Python

- [Logseq Discord #off-topic](https://discord.com/channels/725182569297215569/736514221499744287/967240289888641158)

- [JupyterLite: Jupyter ❤️ WebAssembly ❤️ Python](https://blog.jupyter.org/jupyterlite-jupyter-%EF%B8%8F-webassembly-%EF%B8%8F-python-f6e2e41ab3fa)

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

---

Insert an audio player using the same syntax than images.

- [Logseq Discord #tips](https://discord.com/channels/725182569297215569/740582434961358848/967518539785310228)

`![](assets/audio.mp3)`

---
