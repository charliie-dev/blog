---
title: Logseq | CSS_Theme
description: "A collection of custom.css snippets for styling Logseq: a progress bar, task colours by state, more compact query results, highlighting the block being edited, a custom right sidebar and a query dashboard. It also includes a movable search box, ruby annotations through the logseq-wrap plugin and colouring blocks by tag. Most snippets come from the Logseq Discord and community themes, each linked to its source."
tags:
  - logseq
date: 2022-04-08
lastMod: 2022-04-21
---

Progress par style in `custom.css`:

- [pengx17/logseq-dev-theme: custom.css (L823–L838)](https://github.com/pengx17/logseq-dev-theme/blob/5feb39e5ec8e430a293ffe78736612c666a9806c/custom.css#L823-L838)

```css
/* Progress bar */
progress {
  vertical-align: middle;
  border-radius: 8px;
  height: 0.6em;
  width: 100px;
  border: 1px solid #000;
  overflow: hidden;
}

progress::-webkit-progress-bar {
  background: var(--ls-tertiary-background-color);
}

progress::-webkit-progress-value {
  background: var(--ct-success-color);
}
```

- `id:: 622dec42-eb38-4427-b068-e7b786a82f4f`

---

Custom CSS to move arrows to left - more close to any web-browser layout

```css
/* Toolbar icons */

/* Shrink the left toolbar */
.cp__header > .l {
  width: auto;
}

/* Move all icons to the left */
.cp__header > .r {
  justify-content: flex-start;
}

/* Hide the home button (I don't use it at all) */
body:not([data-page="home"]) .r > div:nth-child(2) {
  display: none !important;
}

/* Add a gap before the plugin icons and reorder them */
.ui-items-container[data-type="toolbar"] {
  margin-left: auto;
  order: 1;
}
.ui__dropdown-trigger {
  order: 2;
}
.r > div:last-child {
  display: flex !important;
  order: 3;
}
```

---

Coloring for tasks according this diagram by @danzu

- [Logseq Discord #themes](https://discord.com/channels/725182569297215569/752845138148982877/953050791298605196)

```css
/* TODO colors */
.form-checkbox:checked {
  background-color: #2c89d9;
}
.block-marker.NOW,
.block-marker.DOING {
  color: #27ae9e;
}
.block-marker.LATER {
  color: #e88438;
}
.block-marker.TODO {
  color: #d3455b;
}
.block-marker.waiting {
  color: #f6c423;
}
```

---

Custom CSS for making query results more compact

- [Logseq Discord #themes](https://discord.com/channels/725182569297215569/752845138148982877/953075525394522173)

```css
/* Compact query results */
.custom-query-title {
  display: flex;
}
.custom-query .color-level,
.custom-query .table-auto {
  margin: 0;
}
.custom-query .flex.flex-row.align-items.mt-2 {
  justify-content: flex-end;
  margin-top: -1.4em;
}
```

---

iOS css: Move the toggle a bit from the edge of the screen and set it to always visible on iOS

- [Logseq Discord #ios-app](https://discord.com/channels/725182569297215569/924907384730689566/953857009311186964)

```css
/* iOS: expand and collapse controls (start) */

/* Position the toggle */
html.is-ios .block-control {
  position: absolute;
  right: 0px;
  margin-right: -14px;
}

/* Toggle size */
html.is-ios .block-control svg {
  width: 24px !important;
  height: 24px !important;
}
/* Rotate the arrow when collapsed */
html.is-ios .block-control .collapsed svg[aria-hidden="true"] {
  transform: rotate(180deg);
}
/* Show the toggle when expanded */
html.is-ios div[haschild="true"] > div > div > .block-control .control-hide {
  display: block;
}

/* Also show the toggle when collapsed */
html.is-ios
  div[data-collapsed="false"]
  > div
  > div
  > .block-control
  .control-hide {
  display: block;
}
html.is-ios .page-blocks-inner {
  padding-left: 10px;
  padding-right: 4px;
}

/* Hide the toggle in flashcards */
html.is-ios .cards-review .ls-card .block-control {
  display: none;
}
/* iOS: expand and collapse controls (end) */
```

---

Dim installed plugins in Marketplace, make installable plugins easier to see (maybe could be a default rule ?)

- [Logseq Discord #themes](https://discord.com/channels/725182569297215569/752845138148982877/958162293366079509)

```css
/* Marketplace: hide installed plugins */
.cp__plugins-marketplace .cp__plugins-item-card.market.installed {
  opacity: 0.4;
}
```

---

Modify the style of the block being edited

- [Logseq Discord #themes](https://discord.com/channels/725182569297215569/752845138148982877/959351405393612800)

```css
.editor-inner textarea {
  background-color: coral;
}
```

---

Custom right sidebar layout

- [Logseq Discord #themes](https://discord.com/channels/725182569297215569/752845138148982877/961641146445078528)

```css
/* Right sidebar layout */
.cp__right-sidebar {
  font-family: "Roboto Slab";
  font-size: 14px;
  background: transparent;
}
div.cp__right-sidebar-topbar {
  background: #80397b;
  margin-top: 7px;
  margin-left: 1px;
  margin-right: 7px;
  border-radius: 4px;
}
.cp__right-sidebar-settings-btn {
  color: white !important;
}
.cp__right-sidebar .sidebar-item {
  background: #faf1f8;
  border-radius: 4px;
  margin-top: 7px;
  margin-right: 7px;
  margin-left: 1px;
  -webkit-box-shadow: 0.7px 0.8px 3px 0px rgba(0, 0, 0, 0.5);
  -moz-box-shadow: 0.7px 0.8px 3px 0px rgba(0, 0, 0, 0.5);
  box-shadow: 0.7px 0.8px 3px 0px rgba(0, 0, 0, 0.5);
}
div.sidebar-item.content.color-level.px-4.shadow-lg {
  padding-top: 0px;
  padding-bottom: 0px;
}
```

![Custom right sidebar layout](/images/logseq/custom-right-sidebar-layout.png)

---

Cool custom query dashboard.

- [Logseq Discord #workflows](https://discord.com/channels/725182569297215569/766475028978991104/961480208823767060)

- [Felipe Ganash on X: Tengo bastante que leer y mucho que procesar](https://twitter.com/FelipeGanash/status/1510504058095816712?s=20&t=jDIBHRLvuQHerXI6LOlBXA)

- Code:

```css
/* Query CSS for better readability */

/* Tip: text behaviour inside the table */
.whitespace-nowrap {
  white-space: initial;
  min-width: 170px;
}

table {
  width: 100%;
  border-collapse: collapse;
  text-align: left;
  margin-left: 0px;
}

/* The whole table container */
.overflow-x-auto {
  min-width: 1100px;
  margin-left: -170px;
}

/* The top of the title */
th {
  font-size: 18px;
  font-weight: 500;
  background-color: var(--ls-secondary-background-color);
  text-transform: capitalize;
  padding: 10px 8px;
}

th .mr-1 {
  color: var(--ls-link-text-color-amarillo);
}

/* Community CSS that compacts the options bar at the top */

.custom-query-title {
  display: flex;
}
.custom-query .color-level,
.custom-query .table-auto {
  margin-top: 5px;
}
.custom-query .flex.flex-row.align-items.mt-2 {
  justify-content: flex-end;
  margin-top: -1.4em;
  position: relative;
  z-index: 1;
}

/* Status CSS */

/* Status: to read */

.page-reference[data-ref^="para leer"] .page-ref,
.page-ref[data-ref^="para leer"] {
  background-color: #e90000;
  color: #fff !important;
  padding: 1px 10px 1px 0px;
  border-radius: 5px;
}

.recent-item[data-ref^="para leer"] a,
.favorite-item[data-ref="Para Leer"] a,
.title[data-ref^="para leer"] {
  color: #transparent;
}

.recent-item[data-ref^="para leer"],
.favorite-item[data-ref="Para Leer"] {
  position: relative;
}
.recent-item[data-ref^="para leer"] .page-icon,
.favorite-item[data-ref="Para Leer"] .page-icon {
  visibility: hidden;
}

.page-reference[data-ref^="para leer"] .page-ref:before,
.page-ref[data-ref^="para leer"]:before {
  content: "🟥 ";
  margin-right: 8px;
  border: 1px solid #fff;
  border-radius: 4px 0px 0px 4px;
}

.recent-item[data-ref^="para leer"]:before,
.favorite-item[data-ref="Para Leer"]:before {
  content: "🟥" !important;
  margin-right: 2px;
  font-size: 14px;
}

.title[data-ref^="para leer"]:before {
  content: "🟥 " !important;
  margin-right: 2px;
}

.recent-item[data-ref^="para leer"]:before,
.favorite-item[data-ref="Para Leer"]:before {
  position: absolute;
  left: 20px;
  top: 3px;
}

/* Status: reading */

.page-reference[data-ref="leyendo"] .page-ref,
.page-ref[data-ref="leyendo"] {
  background-color: #ff8b00;
  color: #fff !important;
  padding: 1px 10px 1px 0px;
  border-radius: 5px;
}

.recent-item[data-ref="leyendo"] a,
.favorite-item[data-ref="Leyendo"] a,
.title[data-ref="leyendo"] {
  color: #transparent;
}

.recent-item[data-ref="leyendo"],
.favorite-item[data-ref="Leyendo"] {
  position: relative;
}
.recent-item[data-ref="leyendo"] .page-icon,
.favorite-item[data-ref="Leyendo"] .page-icon {
  visibility: hidden;
}

.page-reference[data-ref="leyendo"] .page-ref:before,
.page-ref[data-ref="leyendo"]:before {
  content: "🟧 ";
  margin-right: 8px;
  border: 1px solid #fff;
  border-radius: 4px 0px 0px 4px;
}

.recent-item[data-ref="leyendo"]:before,
.favorite-item[data-ref="Leyendo"]:before {
  content: "🟧" !important;
  margin-right: 2px;
  font-size: 14px;
}

.title[data-ref="leyendo"]:before {
  content: "🟧 " !important;
  margin-right: 2px;
}

.recent-item[data-ref="leyendo"]:before,
.favorite-item[data-ref="Leyendo"]:before {
  position: absolute;
  left: 20px;
  top: 3px;
}

/* Status: studying */

.page-reference[data-ref="estudiando"] .page-ref,
.page-ref[data-ref="estudiando"] {
  background-color: #0066fc;
  color: #fff !important;
  padding: 1px 10px 1px 0px;
  border-radius: 5px;
}

.recent-item[data-ref="estudiando"] a,
.favorite-item[data-ref="Estudiando"] a,
.title[data-ref="estudiando"] {
  color: #transparent;
}

.recent-item[data-ref="estudiando"],
.favorite-item[data-ref="Estudiando"] {
  position: relative;
}
.recent-item[data-ref="estudiando"] .page-icon,
.favorite-item[data-ref="Estudiando"] .page-icon {
  visibility: hidden;
}

.page-reference[data-ref="estudiando"] .page-ref:before,
.page-ref[data-ref="estudiando"]:before {
  content: "🟦 ";
  margin-right: 8px;
  border: 1px solid #fff;
  border-radius: 4px 0px 0px 4px;
}

.recent-item[data-ref="estudiando"]:before,
.favorite-item[data-ref="Estudiando"]:before {
  content: "🟦" !important;
  margin-right: 2px;
  font-size: 14px;
}

.title[data-ref="estudiando"]:before {
  content: "🟦 " !important;
  margin-right: 2px;
}

.recent-item[data-ref="estudiando"]:before,
.favorite-item[data-ref="Estudiando"]:before {
  position: absolute;
  left: 20px;
  top: 3px;
}

/* Status: analysed, with notes */

.page-reference[data-ref="analizado - notas"] .page-ref,
.page-ref[data-ref="analizado - notas"] {
  background-color: #ca24ff;
  color: #fff !important;
  padding: 1px 10px 1px 0px;
  border-radius: 5px;
}

.recent-item[data-ref="analizado - notas"] a,
.favorite-item[data-ref="Analizado - Notas"] a,
.title[data-ref="analizado - notas"] {
  color: #transparent;
}

.recent-item[data-ref="analizado - notas"],
.favorite-item[data-ref="Analizado - Notas"] {
  position: relative;
}
.recent-item[data-ref="analizado - notas"] .page-icon,
.favorite-item[data-ref="Analizado - Notas"] .page-icon {
  visibility: hidden;
}

.page-reference[data-ref="analizado - notas"] .page-ref:before,
.page-ref[data-ref="analizado - notas"]:before {
  content: "🟪 ";
  margin-right: 8px;
  border: 1px solid #fff;
  border-radius: 4px 0px 0px 4px;
}

.recent-item[data-ref="analizado - notas"]:before,
.favorite-item[data-ref="Analizado - Notas"]:before {
  content: "🟪" !important;
  margin-right: 2px;
  font-size: 14px;
}

.title[data-ref="analizado - notas"]:before {
  content: "🟪 " !important;
  margin-right: 2px;
}

.recent-item[data-ref="analizado - notas"]:before,
.favorite-item[data-ref="Analizado - Notas"]:before {
  position: absolute;
  left: 20px;
  top: 3px;
}

/* Status: finished */

.page-reference[data-ref="terminado"] .page-ref,
.page-ref[data-ref="terminado"] {
  background-color: #00bf00;
  color: #fff !important;
  padding: 1px 10px 1px 0px;
  border-radius: 5px;
}

.recent-item[data-ref="terminado"] a,
.favorite-item[data-ref="Terminado"] a,
.title[data-ref="terminado"] {
  color: #transparent;
}

.recent-item[data-ref="terminado"],
.favorite-item[data-ref="Terminado"] {
  position: relative;
}
.recent-item[data-ref="terminado"] .page-icon,
.favorite-item[data-ref="Terminado"] .page-icon {
  visibility: hidden;
}

.page-reference[data-ref="terminado"] .page-ref:before,
.page-ref[data-ref="terminado"]:before {
  content: "🟩 ";
  margin-right: 8px;
  border: 1px solid #fff;
  border-radius: 4px 0px 0px 4px;
}

.recent-item[data-ref="terminado"]:before,
.favorite-item[data-ref="Terminado"]:before {
  content: "🟩" !important;
  margin-right: 2px;
  font-size: 14px;
}

.title[data-ref="terminado"]:before {
  content: "🟩 " !important;
  margin-right: 2px;
}

.recent-item[data-ref="terminado"]:before,
.favorite-item[data-ref="Terminado"]:before {
  position: absolute;
  left: 20px;
  top: 3px;
}

/* Status: reread */

.page-reference[data-ref="releer"] .page-ref,
.page-ref[data-ref="releer"] {
  background-color: #8c512e;
  color: #fff !important;
  padding: 1px 10px 1px 0px;
  border-radius: 5px;
}

.recent-item[data-ref="releer"] a,
.favorite-item[data-ref="Releer"] a,
.title[data-ref="releer"] {
  color: #transparent;
}

.recent-item[data-ref="releer"],
.favorite-item[data-ref="Releer"] {
  position: relative;
}
.recent-item[data-ref="releer"] .page-icon,
.favorite-item[data-ref="Releer"] .page-icon {
  visibility: hidden;
}

.page-reference[data-ref="releer"] .page-ref:before,
.page-ref[data-ref="releer"]:before {
  content: "🟫 ";
  margin-right: 8px;
  border: 1px solid #fff;
  border-radius: 4px 0px 0px 4px;
}

.recent-item[data-ref="releer"]:before,
.favorite-item[data-ref="Releer"]:before {
  content: "🟫" !important;
  margin-right: 2px;
  font-size: 14px;
}

.title[data-ref="releer"]:before {
  content: "🟫 " !important;
  margin-right: 2px;
}

.recent-item[data-ref="releer"]:before,
.favorite-item[data-ref="Releer"]:before {
  position: absolute;
  left: 20px;
  top: 3px;
}
```

![Demo](https://pbs.twimg.com/media/FPZjRvBXMAEbOy3.jpg)

---

With some extra css, we can highlight the `ruby` and make the annotation appear only on hover.

- [Logseq Discord #tips](https://discord.com/channels/725182569297215569/740582434961358848/905775051847122965)

- [Logseq Discord #tips](https://discord.com/channels/725182569297215569/740582434961358848/905778303196098570)

- Adding rubi (small annotations on top) with the logseq-wrap plugin. this is what I added in the plugin setting:

  - [Logseq Discord #tips](https://discord.com/channels/725182569297215569/740582434961358848/963386351703719996)

```
"wrap-ruby": {
    "label": "ruby",
    "binding": "",
    "template": "[:ruby \"$^\" [:rt \"\"]]",
    "icon": "<svg width=\"200px\" height=\"200px\" viewBox=\"0 0 76 76\" xmlns=\"http://www.w3.org/2000/svg\" xmlns:xlink=\"http://www.w3.org/1999/xlink\" version=\"1.1\" baseProfile=\"full\" enable-background=\"new 0 0 76.00 76.00\" xml:space=\"preserve\"><path fill=\"#eeeeee\" fill-opacity=\"1\" stroke-width=\"0.2\" stroke-linejoin=\"round\" d=\"M 31.6666,30.0834L 42.7499,30.0834L 42.7499,33.2501L 42.7499,52.2501L 45.9165,52.2501L 45.9165,57.0001L 31.6666,57.0001L 31.6666,52.2501L 34.8332,52.2501L 34.8332,34.8335L 31.6666,34.8335L 31.6666,30.0834 Z M 38.7917,19C 40.9778,19 42.75,20.7722 42.75,22.9583C 42.75,25.1445 40.9778,26.9167 38.7917,26.9167C 36.6055,26.9167 34.8333,25.1445 34.8333,22.9583C 34.8333,20.7722 36.6055,19 38.7917,19 Z \" /></svg>"
  },
```

- and this is what i added in custom.css courtesy of @canniblox

```css
ruby {
  background-color: #b0bec550;
}
```

---

Moveable search box

- [Logseq Discord #tips](https://discord.com/channels/725182569297215569/740582434961358848/963746504185634868)

```css
.ui__modal-overlay div {
  background: none;
}
.panel-content .cp__palette {
  width: 500px;
  height: 500px;
}
.ui__modal-panel {
  transition: none;
  transform: none;
  margin-top: auto;
  margin-right: auto;
}
.ui__modal-panel .panel-content {
  min-width: 500px;
}

#ui__ac-inner .text-sm.font-medium.flex.items-baseline,
.ml-2 {
  font-family: "Segoe UI";
  font-size: 13px;
  font-weight: 550;
  line-height: 1rem;
}
.cp__palette-input,
.cp__select-input {
  font-size: 16px;
  outline: 0;
  outline-color: initial;
  outline-style: initial;
  outline-width: 0px;
}
```

---

To remove/hide/format "Table View", "Set properties", etc. in query results.

- [Logseq Discord #look-what-i-built](https://discord.com/channels/725182569297215569/756886540038438992/964005052060672040)

```css
/* Hide "Set properties" */
.text-sm.mr-1 {
  display: none;
}

/* Hide "table view" from queries */
div.mx-2 {
  display: none;
}

/* Hide the table-view selector */
span.wrapper.transition-colors.ease-in-out {
  display: none;
}

/* Remove the actual query displayed in simple query tables */
div.dsl-query > div.custom-query > div.flex > div.content {
  display: none;
}

/* Format the results count */
span.opacity-60.text-sm.ml-2.results-count {
  font-size: 0.7rem;
  font-weight: 400;
}
```

---

Use `[data-ref="value"]` to modify the CSS attributes(icon, font) for specific tags. For example the following will apply the following changes to tags named "literature note".

- [Logseq Discord #themes](https://discord.com/channels/725182569297215569/752845138148982877/966165573627150357)

- [Logseq Discord #themes](https://discord.com/channels/725182569297215569/752845138148982877/966165913621647410)

- [Logseq Discord #themes](https://discord.com/channels/725182569297215569/752845138148982877/966181728186163251)

```css
a.tag[data-ref$="literature note"]:after {
  content: "📝";
  font-size: 16px;
}
```

```css
a.tag[data-ref$="TAG_NAME"] {
  font-family: "FAMILY_NAME";
}
```

```css
span[data-ref^="Go Live"] .page-ref::before {
  content: "🚀";
}
```

---

Colored block via tags.

- [Logseq Discord #tips](https://discord.com/channels/725182569297215569/740582434961358848/967293385062903859)

```css
/* Tag: note */

div[data-refs-self*="1note"] {
  background-color: rgba(253, 245, 255, 1);
  color: #834b86;
  border: solid 1px #e6c5e8;
  border-left: solid 4px #935496;
  padding: 12px 0px 12px 0px;
  border-radius: 0px 5px 5px 0px;
  margin-top: 5px;
  margin-left: 1px;
  margin-bottom: 10px;
}

a.tag[data-ref="1note"] {
  olor: transparent;
  font-size: 0px;
}

a.tag[data-ref="1note"]:before {
  background-color: #835885;
  bottom: -8px;
  content: "";
  display: inline-block;
  left: -2px;
  margin: -13px 0px 0px 0px;
  mask-image: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAB2AAAAdgB+lymcgAAABl0RVh0U29mdHdhcmUAd3d3Lmlua3NjYXBlLm9yZ5vuPBoAAAQ7SURBVHic7Zvbi01RHMc/DBlEGJchcsnlQUJTo1wGcUhCuTzgxYOU/AHePCGv8qQpbx5QihdxXIZIJiJyaUJEqHFnmIyZ8fBbq7PPvq69Z885++Jbq7X3uq/f/q3v+q3fWQf+I98YUOH+bgDDDMotBzoM2xwONAEtwO9ow6ocfgC9BmGkTxs1QAOwHygCnarO2igDGhSlUgxoAn65pF9Hvqgds4ACsBpYCYxyKVMALoYdSLUE8ADRBju6VTwW+aKrgTXANFu5l8BlFXqBM6ps4qGXwAiP/G8qv4fyJdEOnAJ24xRGLcIXPcCE2EccM0wF0AXcRNZ5AzAwoN2Lqt72sAMK2gVaDToPg4WqvZG4L4FvKu8p7hzhhSnAeOA98M4lfy3wKdRIFboxY+2wIUgD4g71XhM0JcFGZI31Ff1hB/jhEjCmLw1oDaiJYTBgzgF+dkAYfCAmDYgb5yhteVaYaEesqJYAVlapXwfsAhgELKJkdcW5AwBsxGw5hdkBYsFs4DzwHXcWjYsDKo1ADtCop2R9PQOOAZuInwQrDWMBADxUhVdY0jIvACsHFIF5yPpvCWj4AtUjUD9sRbbSSFiHSOuOJc1LA/7QPxZbX8M42zhDacANNbEGYDTwxasSIqxKe5NMEPnra7QgEtus3jPPAfZ9vqjiQn+NKGmwE1kROEiwdyWTJAii6p8QtZlOzkgQNeEWhAP8tCCzJAiwF5HaKXJAgm6YqSp9JAcCcCOy58ArnN5XKzJLghrN+J8G00aCTV4T9fqKRcQH74W0kWABsXSNUUe5RzjtHHA7SuV7ZEcAf5HzjQN+Lq/L/TGiKqEGcbU74CeAok9eGhH6fFNLyU2W9iXQC7RFaSArhlCXiqfaC8Tt9k4q7qrYcb7JiwA0oYe+RJGVJdCk4nZsHz0vGtCGnG/GAvOtGXkRAMBVFZctgzwJwNXfmcQjrQnO4n3HwAqr+XsFsWuWIjZOp0lHSSVB7bc0Ddohcl+9r9INpVUDNLYgv2jbsRPY5ZJeBBYgy+CKSQdJ14A6l7z1lCw/PX6tAQX1rg2jzJHgYuA0otmHkH1fYyIwQz0vRLbEQKRJA+Za0psRj5U2hNpw8sIWk47SIoDJwGuVdp4St1lPgx0IB+jbp0YuvTQIoA54ot6vA0Mt5bQANgCDo3SUdAFMBm6p50c43V6RfhixIukCuKnil8Akl3KZF4A+4c3xKBfbTdFW1VBSYL1K2wWc9CgXeE84iAm7yYatMBHRBgeCNKDRJW0NcBi5TLXPoPMhwBLkGLqMcpYG+AxcQ7w29zC7lT6NcB/ma4iygZiEDPI73lvLDGAPYpXZ/wPQhZiiRxChRNqeqo3HyGSWqfd6YBtwHHiL0+p6ofK2Ed9V+KriKDKxVuRqrX3Cb4ATwA5S8EemKNhA+YR/EsHMTDNGIGbnAeQElna/wn/kFv8A8zvyPfywxCQAAAAASUVORK5CYII=");
  -webkit-mask-box-image: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAB2AAAAdgB+lymcgAAABl0RVh0U29mdHdhcmUAd3d3Lmlua3NjYXBlLm9yZ5vuPBoAAAQ7SURBVHic7Zvbi01RHMc/DBlEGJchcsnlQUJTo1wGcUhCuTzgxYOU/AHePCGv8qQpbx5QihdxXIZIJiJyaUJEqHFnmIyZ8fBbq7PPvq69Z885++Jbq7X3uq/f/q3v+q3fWQf+I98YUOH+bgDDDMotBzoM2xwONAEtwO9ow6ocfgC9BmGkTxs1QAOwHygCnarO2igDGhSlUgxoAn65pF9Hvqgds4ACsBpYCYxyKVMALoYdSLUE8ADRBju6VTwW+aKrgTXANFu5l8BlFXqBM6ps4qGXwAiP/G8qv4fyJdEOnAJ24xRGLcIXPcCE2EccM0wF0AXcRNZ5AzAwoN2Lqt72sAMK2gVaDToPg4WqvZG4L4FvKu8p7hzhhSnAeOA98M4lfy3wKdRIFboxY+2wIUgD4g71XhM0JcFGZI31Ff1hB/jhEjCmLw1oDaiJYTBgzgF+dkAYfCAmDYgb5yhteVaYaEesqJYAVlapXwfsAhgELKJkdcW5AwBsxGw5hdkBYsFs4DzwHXcWjYsDKo1ADtCop2R9PQOOAZuInwQrDWMBADxUhVdY0jIvACsHFIF5yPpvCWj4AtUjUD9sRbbSSFiHSOuOJc1LA/7QPxZbX8M42zhDacANNbEGYDTwxasSIqxKe5NMEPnra7QgEtus3jPPAfZ9vqjiQn+NKGmwE1kROEiwdyWTJAii6p8QtZlOzkgQNeEWhAP8tCCzJAiwF5HaKXJAgm6YqSp9JAcCcCOy58ArnN5XKzJLghrN+J8G00aCTV4T9fqKRcQH74W0kWABsXSNUUe5RzjtHHA7SuV7ZEcAf5HzjQN+Lq/L/TGiKqEGcbU74CeAok9eGhH6fFNLyU2W9iXQC7RFaSArhlCXiqfaC8Tt9k4q7qrYcb7JiwA0oYe+RJGVJdCk4nZsHz0vGtCGnG/GAvOtGXkRAMBVFZctgzwJwNXfmcQjrQnO4n3HwAqr+XsFsWuWIjZOp0lHSSVB7bc0Ddohcl+9r9INpVUDNLYgv2jbsRPY5ZJeBBYgy+CKSQdJ14A6l7z1lCw/PX6tAQX1rg2jzJHgYuA0otmHkH1fYyIwQz0vRLbEQKRJA+Za0psRj5U2hNpw8sIWk47SIoDJwGuVdp4St1lPgx0IB+jbp0YuvTQIoA54ot6vA0Mt5bQANgCDo3SUdAFMBm6p50c43V6RfhixIukCuKnil8Akl3KZF4A+4c3xKBfbTdFW1VBSYL1K2wWc9CgXeE84iAm7yYatMBHRBgeCNKDRJW0NcBi5TLXPoPMhwBLkGLqMcpYG+AxcQ7w29zC7lT6NcB/ma4iygZiEDPI73lvLDGAPYpXZ/wPQhZiiRxChRNqeqo3HyGSWqfd6YBtwHHiL0+p6ofK2Ed9V+KriKDKxVuRqrX3Cb4ATwA5S8EemKNhA+YR/EsHMTDNGIGbnAeQElna/wn/kFv8A8zvyPfywxCQAAAAASUVORK5CYII=");
  mask-position: center;
  -webkit-mask-box-position: center;
  mask-repeat: no-repeat;
  -webkit-mask-box-repeat: no-repeat;
  mask-size: 24px 24px;
  -webkit-mask-box-size: 14px 14px;
  object-fit: cover;
  position: relative;
  transition: 0.3s;
  width: 1.8rem;
  height: 1.8rem;
}

.dark div[data-refs-self*="1note"] {
  background-color: rgba(91, 33, 182, 1);
  background-image: url(data:image/png;base64,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);
  color: #d6cdfc;
  border: solid 1px #8b5cf6;
  border-left: solid 4px #8b5cf6;
}

.dark a.tag[data-ref="1note"]:before {
  background-color: #ede9fe;
}
```

---

Good Images for questions and answers, and turn icons into base64.

- [Logseq Discord #tips](https://discord.com/channels/725182569297215569/740582434961358848/967459249171071056)

- [Logseq Discord #tips](https://discord.com/channels/725182569297215569/740582434961358848/967462539581603980)

```css
/* Questions, answers, notes, ideas, executed ideas and claims */

div[data-refs-self*="1pregunta"] {
  background-color: rgba(255, 245, 245, 1);
  color: #721c24;
  border: solid 1px #f5c6cb;
  border-left: solid 4px;
  padding: 12px 0px 12px 0px;
  border-radius: 0px 5px 5px 0px;
  margin-top: 5px;
  margin-left: 1px;
  margin-bottom: 10px;
}

a.tag[data-ref="1pregunta"] {
  color: transparent;
  font-size: 0px;
}

a.tag[data-ref="1pregunta"]:before {
  background-color: #721c24;
  bottom: -8px;
  content: "";
  display: inline-block;
  left: -2px;
  margin: -13px 0px 0px 0px;
  mask-image: url("data:image/png;base64,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");
  -webkit-mask-box-image: url("data:image/png;base64,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");
  mask-position: center;
  -webkit-mask-box-position: center;
  mask-repeat: no-repeat;
  -webkit-mask-box-repeat: no-repeat;
  mask-size: 24px 24px;
  -webkit-mask-box-size: 14px 14px;
  object-fit: cover;
  position: relative;
  transition: 0.3s;
  width: 1.8rem;
  height: 1.8rem;
}

.dark div[data-refs-self*="1pregunta"] {
  background-color: rgba(153, 27, 27, 1);
  background-image: var(--bg-imagen-postit);
  color: #ffcfcf;
  border: solid 1px #ef4444;
  border-left: solid 4px #ef4444;
}

.dark a.tag[data-ref="1pregunta"]:before {
  background-color: #fee2e2;
}

/* Dark theme: translucent red left border */
div[data-refs-self*="1pregunta"] .block-children-left-border {
  width: 3px;
  background-color: #ef4444;
  border-radius: 6px;
  left: 0px;
  top: 0;
  height: 100%;
  cursor: pointer;
  background-clip: content-box;
  position: absolute;
  opacity: 70%;
}

/* Dark theme: left border when selected */
div[data-refs-self*="1pregunta"] .block-children-left-border:hover {
  background-color: var(--ls-link-text-color-amarillo);
}

/* Dark theme: red bullet for this block */
div[data-refs-self*="1pregunta"] .bullet-container .bullet {
  background-color: #ef4444 !important;
  opacity: 50%;
}

/* Tag: answers */

div[data-refs-self*="1respuesta"] {
  background-color: rgba(245, 255, 245, 1);
  color: #155724;
  border: solid 1px #c3e6cb;
  border-left: solid 4px;
  padding: 12px 0px 12px 0px;
  border-radius: 0px 5px 5px 0px;
  margin-top: 5px;
  margin-left: 1px;
  margin-bottom: 10px;
}

a.tag[data-ref="1respuesta"] {
  olor: transparent;
  font-size: 0px;
}

a.tag[data-ref="1respuesta"]:before {
  background-color: #155724;
  bottom: -8px;
  content: "";
  display: inline-block;
  left: -2px;
  margin: -13px 0px 0px 0px;
  mask-image: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAB2AAAAdgB+lymcgAAABl0RVh0U29mdHdhcmUAd3d3Lmlua3NjYXBlLm9yZ5vuPBoAAARDSURBVHic7dpbiFVVHMfxj2NaKhmhpRVO0Q26DVhEmV3o9lTRBSJ6q4fCfAh6jSLqoRtdIFKhAnsI6qEgyOklpB6SrCxS6sXKCitS07LS1BxPD/+1O7vDnDnn7H1m9ozuL2xm9j7r/1+//T9r7/Vf/3WoqampqampOVKZ1kPbE3A2Zo2Tln7xNzZjR78cLsBbGEFjihwjeDNpH5NOI+B4fIyzsB+bsLuT04o5DkM4WoyES/B7UWfPiIh+gcF+qJsgTsVGof3pMo5+SE4u64OoiWap0P5dGScHk5MZ/VA0wcwQ2g+WcZK9VKYqHfUfNUFC8swWL6bFOFEI/Alf4UP8U4GmtvRzBCzCSvFGbjd9/YYXcHKf+uyov9M0mBn3kjCNxv14HHPS+Qasx48YEMFZKqYv2IsH8FLJfkvrLzsCBrAq5+d1nDNG+3PxRq79SuWCX3oEl3XwWLLfg9t6sLs92TTwRIn+Kw3AVTgkXmrXFbC/XmSfI8lXESoLwDTxnDfwUIn+H9bMRItQWQCuSHZbcUwPdqfhytz5TDFFNnB1AR0d9Q8UcNoNd6a/q7GvS5shfIoPNNcdB/BK+v+WfonrhaIjIBv+S7psPyTW7w0MY3rus8vT9c8L6KjsEfgz2c3NXTtDLFFbGcJ2zZtvbTM/fba9gI7KAnAg2WWp9mIxI6wX6/WMTjdPvAcaYkbolcoCsDvZzUvn87AlXcuCkL/5NUa/eViY2vxSQEdlAVif7PLz96BmEDbo7uaJfKCBjwroqCwAzya751uuL8I3Ob/v6jxNPpfaPlVAR2UBuDjZ7RB1xTyD+EysC8b65uFY7Eq+Liygo9JU+L1ku6pE/y8mH+8XtK80AEOiRt/AsgL2C8Q6YD/OL6ih8tXgXcn+EB71/wSnEwPiub+jRP+VB4D49rPi6kaR0s4cpd0AbhAbGveW7DNj0lSErsHLOD2d7xJTYVZ2P0mkzfPT52twU8k+mQQVoTyzxGjYlPPbemwWpbC5bXz0yqQZAa2cIqbKhSJd3iaSp6197mdSjYAqKF0PyHZVJvuW+GhkmsfcZ+gUgC3pb9GaXJVkFaRvyzjJanLf49KSgiaSJZozzINjNez0cpiFtZqVnV/xR1l1HfgS9+iuAHKBWFPkH9G5mtPpOlyrWC3hP2aLPfad2k9f/T6+xpldaLuvjf1OkUV2fHf1Mj1MF/l5L1XeXpmNV3GRGAE3ikJpO5ZjBV7DI+naPjGtjoybynFmjsgEG/hLBKEdy1O7FUU7G6+yeBn24GaxMToHbyu2muyKyRgAYvgu01xBrsKT+p+RTgnuFslMQ2y05H+uc1g+Aq2sFjvLe0V9YViUyvrCVAgAvCMyu+2iSrxW/LymNFMlAPCJ2HTdIlaS68Ru0xHHAs29x+w4rN8BrWwTi7PhfjibigEgcoVbRdYIP1cnpXrO01u1uaampqampqYG/AtQpogrPsY06wAAAABJRU5ErkJggg==");
  -webkit-mask-box-image: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAB2AAAAdgB+lymcgAAABl0RVh0U29mdHdhcmUAd3d3Lmlua3NjYXBlLm9yZ5vuPBoAAARDSURBVHic7dpbiFVVHMfxj2NaKhmhpRVO0Q26DVhEmV3o9lTRBSJ6q4fCfAh6jSLqoRtdIFKhAnsI6qEgyOklpB6SrCxS6sXKCitS07LS1BxPD/+1O7vDnDnn7H1m9ozuL2xm9j7r/1+//T9r7/Vf/3WoqampqampOVKZ1kPbE3A2Zo2Tln7xNzZjR78cLsBbGEFjihwjeDNpH5NOI+B4fIyzsB+bsLuT04o5DkM4WoyES/B7UWfPiIh+gcF+qJsgTsVGof3pMo5+SE4u64OoiWap0P5dGScHk5MZ/VA0wcwQ2g+WcZK9VKYqHfUfNUFC8swWL6bFOFEI/Alf4UP8U4GmtvRzBCzCSvFGbjd9/YYXcHKf+uyov9M0mBn3kjCNxv14HHPS+Qasx48YEMFZKqYv2IsH8FLJfkvrLzsCBrAq5+d1nDNG+3PxRq79SuWCX3oEl3XwWLLfg9t6sLs92TTwRIn+Kw3AVTgkXmrXFbC/XmSfI8lXESoLwDTxnDfwUIn+H9bMRItQWQCuSHZbcUwPdqfhytz5TDFFNnB1AR0d9Q8UcNoNd6a/q7GvS5shfIoPNNcdB/BK+v+WfonrhaIjIBv+S7psPyTW7w0MY3rus8vT9c8L6KjsEfgz2c3NXTtDLFFbGcJ2zZtvbTM/fba9gI7KAnAg2WWp9mIxI6wX6/WMTjdPvAcaYkbolcoCsDvZzUvn87AlXcuCkL/5NUa/eViY2vxSQEdlAVif7PLz96BmEDbo7uaJfKCBjwroqCwAzya751uuL8I3Ob/v6jxNPpfaPlVAR2UBuDjZ7RB1xTyD+EysC8b65uFY7Eq+Liygo9JU+L1ku6pE/y8mH+8XtK80AEOiRt/AsgL2C8Q6YD/OL6ih8tXgXcn+EB71/wSnEwPiub+jRP+VB4D49rPi6kaR0s4cpd0AbhAbGveW7DNj0lSErsHLOD2d7xJTYVZ2P0mkzfPT52twU8k+mQQVoTyzxGjYlPPbemwWpbC5bXz0yqQZAa2cIqbKhSJd3iaSp6197mdSjYAqKF0PyHZVJvuW+GhkmsfcZ+gUgC3pb9GaXJVkFaRvyzjJanLf49KSgiaSJZozzINjNez0cpiFtZqVnV/xR1l1HfgS9+iuAHKBWFPkH9G5mtPpOlyrWC3hP2aLPfad2k9f/T6+xpldaLuvjf1OkUV2fHf1Mj1MF/l5L1XeXpmNV3GRGAE3ikJpO5ZjBV7DI+naPjGtjoybynFmjsgEG/hLBKEdy1O7FUU7G6+yeBn24GaxMToHbyu2muyKyRgAYvgu01xBrsKT+p+RTgnuFslMQ2y05H+uc1g+Aq2sFjvLe0V9YViUyvrCVAgAvCMyu+2iSrxW/LymNFMlAPCJ2HTdIlaS68Ru0xHHAs29x+w4rN8BrWwTi7PhfjibigEgcoVbRdYIP1cnpXrO01u1uaampqampqYG/AtQpogrPsY06wAAAABJRU5ErkJggg==");
  mask-position: center;
  -webkit-mask-box-position: center;
  mask-repeat: no-repeat;
  -webkit-mask-box-repeat: no-repeat;
  mask-size: 24px 24px;
  -webkit-mask-box-size: 14px 14px;
  object-fit: cover;
  position: relative;
  transition: 0.3s;
  width: 2.2rem;
  height: 2.2rem;
}

.dark div[data-refs-self*="1respuesta"] {
  background-color: rgba(6, 95, 70, 1);
  background-image: var(--bg-imagen-postit);
  color: #b8f4d5;
  border: solid 1px #10b981;
  border-left: solid 4px #10b981;
}

.dark a.tag[data-ref="1respuesta"]:before {
  background-color: #c8f8df;
}

/* Dark theme: left border for 1respuesta */
div[data-refs-self*="1respuesta"] .block-children-left-border {
  width: 3px;
  background-color: #10b981;
  border-radius: 6px;
  left: 0px;
  top: 0;
  height: 100%;
  cursor: pointer;
  background-clip: content-box;
  position: absolute;
  opacity: 70%;
}

/* Dark theme: left border when 1respuesta is selected */
div[data-refs-self*="1respuesta"] .block-children-left-border:hover {
  background-color: var(--ls-link-text-color-amarillo);
}

/* Dark theme: bullet for 1respuesta blocks */
div[data-refs-self*="1respuesta"] .bullet-container .bullet {
  background-color: #10b981 !important;
  opacity: 50%;
}
```

---

A funny experiment to turn blocks / children-blocks into resizable blocks that behave like flex-boxes

- [Logseq Discord #themes](https://discord.com/channels/725182569297215569/752845138148982877/951186890328002570)

```css
/* ls-blocks: resizable children blocks with depth levels */
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
