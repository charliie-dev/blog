---
title: PKM | Choosing and Combining Note Tools
description: "How I think about note-taking tools, from my notes on several videos: how to pick a tool and then commit to it, why Logseq works for technical notes, splitting the work between Logseq and Obsidian over one Markdown folder, a Zotero, LiquidText and Logseq reading pipeline, simple queries, and taking notes on YouTube videos in Logseq. Sources are listed at the end."
tags:
  - logseq
  - pkm
date: 2022-03-09
lastMod: 2026-10-06T00:00:00+08:00
---

What I took away from a handful of videos about personal knowledge management (PKM) tools: how to
choose one, why Logseq, and how it fits with other tools. The lines I keep coming back to are in
[Logseq | Mottos]({{< relref "Logseq/Mottos.md" >}}). Sources are collected at the bottom.

## Choosing a tool

Tools on Tech's checklist:[^choose]

1. **Do you need a tool at all?** A tool adds to the system you already have. Without a working
   system, a new tool will not fix the problem.
2. **What problem should it solve?** Write down the pros and cons of how you work today.
3. **Which tools fit?** Watch a few reviews, then ask whether a tool suits you: not only its
   features, but whether it feels good to use.
4. **Test it.** Make a free account and check that it does the core job, the deal-breakers you
   cannot live without.
5. **Commit.** No tool is perfect. Decide what you can and cannot live with, then stay with it;
   switching every month is not where happiness lies.

There is no best tool, only the one that fits your own list of needs.

## Why Logseq for technical notes

Bryan Jenks keeps his technical notes and developer log in Logseq, separate from his other
notes.[^devlog]

| Likes                                          | Dislikes, but not deal-breakers |
| ---------------------------------------------- | ------------------------------- |
| A strict outliner                              | A flood of Git commits          |
| Private: you own your data                     | An odd query syntax             |
| Versioned and synced through GitHub            |                                 |
| Plain-text Markdown                            |                                 |
| Built on web technology, so CSS can restyle it |                                 |

## Logseq and Obsidian together

Both apps read the same folder of Markdown files, so they can share one set of notes, synced to
other devices through GitHub.[^both-tot]

![Obsidian and Logseq both reading one folder of Markdown files, synced through GitHub to a phone and a laptop](/images/pkm/logseq-obsidian-github.png)

Santi Younger frames the split with Simon Sinek's Golden Circle: start from **why** you want
something, then **how**, then **what**.[^both-sy]

![The Golden Circle: why at the centre, then how, then what](/images/pkm/golden-circle.png)

| Obsidian is better at                | Logseq is better at                         |
| ------------------------------------ | ------------------------------------------- |
| Community plugins                    | Daily notes                                 |
| Writing long-form text               | Embedding blocks                            |
| Speed and stability                  | Backlinks                                   |

So the **why** is to use each tool for its strength, and the **what** follows: make outlines in
Logseq, and write the detailed paragraphs in Obsidian. Tools on Tech reaches the same split:
Obsidian for long documents, Logseq for pulling blocks from many notes onto one page.[^both-tot]

## A reading pipeline: Zotero, LiquidText and Logseq

A workflow someone shared in the Logseq community, on a Mac and an iPad:

1. **Zotero** keeps every reference, with its PDF attached.
2. **LiquidText** opens those Zotero-linked PDFs for highlighting, then turns the highlights into
   a unified summary.
3. **Logseq** gets the short, clean summary notes, each linked back to Zotero through Logseq's
   Zotero plugin. That reads better across apps than highlighting inside Logseq's own PDF reader.
4. **Obsidian** serves as a light editor on the go, for article notes on pages first set up in
   Logseq, and for long-form and non-academic writing.

Two details from the thread: LiquidText's Zotero integration comes with the one-time Pro purchase,
no subscription needed, and its Zotero sync is limited by your Zotero cloud storage, which is
300 MB on the free plan.

![A PDF highlighted in LiquidText next to the summary notes in Logseq](/images/pkm/liquidtext-in-logseq.jpg)

## Simple queries

Queries beat search for two reasons: they find exactly what you describe, and they stay on the
page as a permanent record you can return to.[^queries]

| Query                                           | Finds                                               |
| ----------------------------------------------- | --------------------------------------------------- |
| `{{query (and [[a]] [[b]])}}`                   | Blocks that reference both pages                    |
| `{{query (or [[a]] [[b]])}}`                    | Blocks that reference either page                   |
| `{{query (and [[a]] (not [[b]]))}}`             | Blocks that reference `a` but not `b`               |
| `{{query (property name value)}}`               | Blocks with a property set to a value               |
| `{{query (todo now later)}}`                    | Tasks in the given states                           |

Queries can also sit in a Markdown table, for example to compare wake-up and bed times side by
side. For advanced queries, see [Logseq | Queries]({{< relref "Logseq/Queries.md" >}}).

## Taking notes on videos

All of the notes above started as video notes in Logseq. Three things make that easier:[^video]

- **`Ctrl+Shift+Y`** inserts a timestamp for the embedded YouTube video's current position.
- **A taller player.** This line in `custom.css` makes the embedded player easier to watch:

  ```css
  iframe[id^="youtube-player"] {
    height: 700px !important;
  }
  ```

- **Readable timestamps.** Logseq's timestamp macros can be turned into plain Markdown, so the notes
  stay readable outside Logseq.

[^choose]: [Tools on Tech: 053 LoY What Tool to select Items of power](https://youtu.be/hPZnsfIQbSY)
[^devlog]: [Bryan Jenks: How I'm Using Logseq For My DevLog & Technical Notes](https://youtu.be/43PKm0TfyNk)
[^both-tot]: [Tools on Tech: How to use Obsidian and Logseq together and why a Markdown backend matters](https://youtu.be/knxDHO3U2_8)
[^both-sy]: [Santi Younger: Obsidian and Logseq - Why Use Both?](https://youtu.be/WpnbSWt_mgM)
[^queries]: [OneStutteringMind: How to use queries and indentation in Logseq (with examples)](https://youtu.be/qQ8DzumRZkM)
[^video]: [aadimator: Taking notes on YouTube videos in LogSeq](https://youtu.be/PvFr36bcpYc)
