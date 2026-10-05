# Logseq | Queries

Notes on Logseq's advanced queries. The first part covers the background and where to learn it;
the rest are working queries grouped by what they find. Each section says what the query does,
then shows it; sources are collected at the bottom.

## Background

### Terms

Collected by @Jeddychan and @Bad3r on the Logseq Discord:

| Term       | What it is                                                                                                     |
| ---------- | -------------------------------------------------------------------------------------------------------------- |
| Datalog    | A rule-based query language for databases, around since the 1980s.                                             |
| Datascript | A flavour of Datalog written in Clojure. Logseq uses it.                                                       |
| Datomic    | Another flavour of Datalog written in Clojure. Some of its tutorials help, though not everything applies.      |
| Hiccup     | HTML written as Clojure data. An advanced query's custom view (`:view`) returns Hiccup.                        |

### Reading list

- **Logseq:** [Logseq Docs: Queries](https://logseq.github.io/#/page/queries),
  [Logseq Docs: Advanced Queries](https://docs.logseq.com/#/page/advanced%20queries) and
  [Logseq Docs: Hiccup](https://logseq.github.io/#/page/hiccup).
- **Logseq, unofficial:** [Logseq MSK Docs: Queries](https://mschmidtkorth.github.io/logseq-msk-docs/#/page/queries)
  and its [Queries/Advanced Queries/Tutorial](https://mschmidtkorth.github.io/logseq-msk-docs/#/page/Queries%2FAdvanced%20Queries%2FTutorial).
- **Logseq's database schema:** [logseq/logseq: src/main/frontend/db_schema.cljs](https://github.com/logseq/logseq/blob/master/src/main/frontend/db_schema.cljs)
  shows how the database is structured and lists every keyword, many of which appear in the
  queries below.
- **Datalog:** [Learn Datalog Today!](http://www.learndatalogtoday.org/), starting with its
  chapter on [Extensible Data Notation](http://www.learndatalogtoday.org/chapter/0), and
  [Learn Crux Datalog Today](https://nextjournal.com/try/learn-crux-datalog-today/learn-crux-datalog-today).
  Reading query keywords as relationships between objects makes them far more intuitive.
- **Datascript:** [DataScript Wiki: Getting started](https://github.com/tonsky/datascript/wiki/Getting-started),
  [tonsky/datascript - Resources](https://github.com/tonsky/datascript#resources) and, for the
  `:keys` option, [DataScript queries - Return maps](https://github.com/tonsky/datascript/blob/master/docs/queries.md#return-maps).
- **Datomic:** [Datomic Query](https://docs.datomic.com/query.html). Only part of it applies, but
  [Query Reference - Syntax Used In Grammar](https://docs.datomic.com/on-prem/query/query.html#syntax-used-in-grammar)
  explains what all the brackets (`[ ]`, `{ }`, `( )`) mean.
- **Hiccup:** [Hiccup Wiki: Syntax](https://github.com/weavejester/hiccup/wiki/Syntax) and
  [yokolet/hiccup-samples](https://github.com/yokolet/hiccup-samples).

### Simple query rules inside advanced queries

Advanced queries can call the rules behind simple queries, such as `page-property`, as
@cldwalker pointed out.[^rules][^rules-example] The rules share their names with the simple query
operators, but their arguments can differ; a gist lists examples of each.[^rules-gist] The release
date queries below use `page-property` this way.

## Tags and references

### List every block's UUID

Shows the raw result as text, which is handy for seeing what a query returns:

```clojure
#+BEGIN_QUERY
{:title "tmp"
 :view (fn [result] (for [r result] [:pre (pr-str r)]))
 :query [:find (pull ?b [*])
         :where
         [?b :block/uuid _]]}
#+END_QUERY
```

### Pages with a given tag

A corrected version of example 6, "All pages have a programming tag", from the Logseq
documentation.[^docs-advanced] It lists the pages as links:

```clojure
#+BEGIN_QUERY
{:title "All pages have a *programming* tag"
 :query [:find ?name
         :in $ ?tag
         :where
         [?t :block/name ?tag]
         [?p :page/tags ?t]
         [?p :block/name ?name]]
 :inputs ["programming"]
 :view (fn [result]
         [:div.flex.flex-col
          (for [page result]
            [:a {:href (str "#/page/" page)} (clojure.string/capitalize page)])])}
#+END_QUERY
```

### Journal blocks that reference one of several tags

Finds blocks from the last seven days of journals that reference `TAG1` or `othertag`:

```clojure
#+BEGIN_QUERY
{:title [:h2 "Commitments"]
 :query [:find (pull ?b [*])
         :in $ ?start ?today ?tag
         :where
         [?b :block/page ?p]
         [?p :page/journal-day ?d]
         [(>= ?d ?start)]
         [(<= ?d ?today)]
         [?b :block/ref-pages ?ref]

         (or [?ref :block/name ?tag]
             [?ref :block/name "othertag"])]

 :inputs [:7d-before :today "TAG1"]}
#+END_QUERY
```

### Blocks that reference a page or tag

Finds every block that links to a page or carries it as a tag; the input is the page's lower-case
name:

```clojure
#+BEGIN_QUERY
{:title "Query for page references & tags."
 :query [:find (pull ?b [*])
         :in $ ?page_name
         :where
         [?b :block/refs ?r]
         [?r :block/name ?page_name]]
 :inputs
 ["mar 28, 2022"]}
#+END_QUERY
```

## Properties

### Sort by a block property

Lists the blocks that have a `birthday` property in a table and sorts them by it. The sort key
`:verjaardag` is Dutch for birthday; rename it to match the property you filter on.

```clojure
- query-table:: false
  query-properties:: [:alias :birthday]
  #+BEGIN_QUERY
  {
   :query [:find (pull ?b [*])
        :where
        [?b :block/properties ?bprops]
        [(get ?bprops :birthday "nil") ?bs]
        [(not= ?bs "nil")]]
  :result-transform (fn [result]
      (sort-by (fn [h]
        (get-in h [:block/properties :verjaardag])) result))
  }
  #+END_QUERY
```

### Blocks that point at the current page

**Goal.** Put one query in a page template that always finds the blocks related to the page it
sits on, instead of hard-coding the page name on every page.

**How.** `:current-page` passes the page's name in. This version finds the blocks on the current
page that are tagged `datalog`:

```clojure
#+BEGIN_QUERY
{:query [:find (pull ?b [*])
         :in $ ?current-page
         :where
         [?p :block/name ?current-page]
         [?b :block/page ?p]
         [?b :block/path-refs [:block/name "datalog"]]]
 :inputs [:current-page]}
#+END_QUERY
```

This one finds open tasks whose `projects` property contains the current page, sorted by
priority:

```clojure
#+BEGIN_QUERY
{:query [:find (pull ?e [*])
         :in $ ?current-page
         :where
         [?b :block/marker ?marker]
         [(contains? #{"TODO" "LATER" "NOW" "DOING"} ?marker)]
         [?e :block/properties ?prop]
         [(get ?prop :projects) ?value]
         [(contains? ?value ?current-page)]]
 :inputs [:current-page]
 :result-transform (fn [result]

                     (sort-by (fn [h]
                                (get h :block/priority "Z")) result))
 :collapsed? false}
#+END_QUERY
```

The page name arrives in lower case. `:block/original-name` keeps the original case, but
`:block/name` is always lower case, so do not rely on case to tell pages apart.[^lowercase]

### Pages with an `icon::` property

Finds every block whose properties include `icon`, to pair with the project status tip in
[Logseq | Tips]({{< relref "Tips.md#show-a-projects-status-with-an-icon-property" >}}):[^icon]

```clojure
#+BEGIN_QUERY
 {:title ""
  :query [:find (pull ?p [*])
          :where
          [?p :block/properties ?prop]
          [(get ?prop :icon) ?icon]
          ;; [(= "example" ?type)]
          ]}
#+END_QUERY
```

### Filter by a number range

**Goal.** Find blocks whose numeric property, such as `num-prop`, is above 100 or between 20 and
50.

**How.** Read the property and compare it:[^number]

```clojure
#+BEGIN_QUERY
{:query [:find (pull ?b [*])
         :where
         [?b :block/properties ?prop]
         [(get ?prop :num-prop) ?num]
         [(> ?num 100)]]}
#+END_QUERY
```

For a range, replace `[(> ?num 100)]` with two comparisons:

```clojure
         [(< ?num 50)]
[(> ?num 20)]
```

There is no `float` function, so multiply by `1.0` to compare fractional values:

```clojure
#+BEGIN_QUERY
{:query [:find (pull ?b [*])
         :where
         [?b :block/properties ?prop]
         [(get ?prop :num-prop) ?num]
         [(* 1.0 ?num) ?numf]
         [(< ?numf 50)]
         [(> ?numf 20)]]}
#+END_QUERY
```

### Filter by a release date

**Goal.** List the series still to watch that have not been released yet.

**How.** Store the release date on each page, either as a Unix timestamp in milliseconds or as a
`yyyyMMdd` number:[^release]

- **Ozark/S04/Part 1**

```
title:: Ozark/S04/Part 1
status:: #towatch
type:: series
genre:: Crime, Drama, Thriller
tags:: Netflix
release_timestamp:: 1642723200000
release_smushed:: 20220121
```

- **Ozark/S04/Part 2**

```
title:: Ozark/S04/Part 2
status:: #towatch
type:: series
genre:: Crime, Drama, Thriller
tags:: Netflix
release_timestamp:: 1651190400000
release_smushed:: 20220429
```

Compare the timestamp with `:right-now-ms`:

```clojure
#+BEGIN_QUERY
{:title "Unreleased Shows"
 :query [:find (pull ?p [*])
         :in $ ?today
         :where
         (page-property ?p :type "series")
         (page-property ?p :status "towatch")
         [?p :block/properties ?props]
         [(get ?props :release-timestamp) ?d]
         [(>  ?d ?today)]]
 :inputs [:right-now-ms]}
#+END_QUERY
```

Or compare the `yyyyMMdd` number with `:today`, which uses the same format:

```clojure
#+BEGIN_QUERY
{:title "Unreleased Shows"
 :query [:find (pull ?p [*])
         :in $ ?today
         :where
         (page-property ?p :type "series")
         (page-property ?p :status "towatch")
         [?p :block/properties ?props]
         [(get ?props :release-smushed) ?d]
         [(>  ?d ?today)]]
 :inputs [:today]}
#+END_QUERY
```

### Dates stored as journal links

When the `release` property links to a journal page instead, match the journal page by
`:block/original-name` rather than `:block/name`, because the property keeps the date in the
journal's original format:[^journal-link][^journal-link-2]

```clojure
#+BEGIN_QUERY
{:title ["Unreleased Shows"]
 :query [:find (pull ?p2 [*])
         :in $ ?today
         :where
         [?p2 :block/name _]
         [?p2 :block/properties ?prop]
         [(get ?prop :release) ?rel]
         [?p :block/original-name ?n]
         [(contains? ?rel ?n)]
         [?p :block/journal-day ?d]
         [(> ?d ?today)]
         [(get ?prop :type) ?type]
         [(contains? #{"series"} ?type)]
         [(get ?prop :status) ?status]
         [(= #{"towatch"} ?status)]]
 :inputs [:today]}
#+END_QUERY
```

### Scheduled tasks within a namespace

Finds scheduled tasks on pages under the `golf` namespace:[^namespace]

```clojure
#+BEGIN_QUERY
{:title " Scheduled dates in Golf namespace"
 :query [:find (pull ?b [*])
         :where
         [?b :block/scheduled ?d]
         [?b :block/marker ?marker]
         [?b :block/page ?p]
         [?p :block/namespace ?ns]
         [?ns :block/name ?nsn]
         [(contains? #{"golf"} ?nsn)]]
 :collapsed? false}
#+END_QUERY
```

`:block/namespace` is a page attribute, and `:block/name` is the lower-case form of
`:block/original-name`, the page name.

## Tasks and dates

### Scheduled tasks from today on

Lists scheduled tasks that are not done, cancelled or `LATER`, from today onwards, sorted by
priority:

```clojure
#+BEGIN_QUERY
{:title "Scheduled items"
 :query [:find (pull ?b [*])
         :in $ ?today
         :where

         [?b :block/scheduled]
         [(get-else $ ?b :block/priority "NIL") ?prio]
         [(get-else $ ?b :block/marker "NIL") ?marker]
         [(not= ?marker "DONE")]
         [(not= ?marker "CANCELED")]
         [(not= ?marker "LATER")]

         [(get-else $ ?b :block/scheduled ?today) ?d]
         [(>= ?d ?today)]]

 :inputs [:today]
 :breadcrumb-show? false
 :result-transform (fn [result]
                     (sort-by (fn [h]
                                (get h :block/priority "Z"))
                              result))
 :collapsed? false}
#+END_QUERY
```

### Scheduled or deadline items on that day's journal

Placed in the journal template, it shows only the items scheduled or due on that journal's date:

```clojure
#+BEGIN_QUERY
{:title "SCHEDULED OR DEADLINE"
 :query [:find (pull ?b [*])
         :in $ ?current-page
         :where
         [?p :block/name ?current-page]
         [?p :block/journal? true]
         [?p :block/journal-day ?date]
         (or
          [?b :block/scheduled ?date]
          [?b :block/deadline ?date])
         (not [?b :block/marker])]
 :inputs [:current-page]}
#+END_QUERY
```

### Collect the week's journal entries

Gathers the last seven days of journal blocks, for example into the Friday entry:[^week]

```clojure
#+BEGIN_QUERY
{:title "What has been done"
 :query [:find (pull ?b [*])
         :in $ ?current-page ?start ?today
         :where
         [?b :block/page ?p]
         [?p :page/journal? true]
         [?p :page/journal-day ?d]
         ;; [?p :block/name ?current-page]
         ;; [?p :block/journal-day ?current-page-day]
         [(>= ?d ?start)]
         (or
          [(<= ?d ?current-page)]
          [(<= ?d ?today)])]
 :inputs [:current-page :7d-before :today]}
#+END_QUERY
```

### Recurring tasks

```clojure
#+BEGIN_QUERY
{:query [:find (pull ?b [*])
         :where
         [?b :block/repeated?]]}
#+END_QUERY
```

The query comes from #queries.[^recurring]

### Today's date in a simple query

`{{query <%today%>}}` always queries the current date.[^today]

### A NOW, LATER and WAITING dashboard

A Discord member shared the dashboard they use with a NOW, LATER and WAITING workflow:

> - add something not urgent (and visually "muted") with WAITING without date
> - add common task with LATER + plan it if needed with deadline\scheduled
> - add current\urgent task with NOW --> work on it --> finish with DONE
> - take overdued tasks
> - take some LATER task --> delete planed date if it was --> set it to NOW --> work --> finish
>   with DONE

The dashboard mixes two layers, the task status and the calendar, to answer five questions. The
queries do not sort, so the results stay grouped under their parent pages.

**What is overdue?** Anything scheduled or due before today, plus `NOW` tasks that carry a date,
which in this workflow should not have one:

```clojure
#+BEGIN_QUERY
{:title
 [[:strong "🔥 Overdue"] [:span " or "] [:span.block-marker.NOW "NOW"] [:sup "(with-date)"]]
 :query [:find (pull ?block [*])
         :in $ ?start ?next
         :where
         [?block :block/marker ?m]
         (or-join [?block ?start ?next ?m]
                  (and
                   (or [?block :block/scheduled ?d] [?block :block/deadline ?d])
                   [(> ?d ?start)]
                   [(< ?d ?next)])
                  ;;
                  (and
                   [(contains? #{"NOW"} ?m)]
                   (or [?block :block/scheduled ?d] [?block :block/deadline ?d])))]

 :inputs [:365d-before :today]
 :breadcrumb-show? false
 :collapsed? false}
#+END_QUERY
```

**What should I do today?** `NOW` tasks without a date, and `LATER` tasks dated today:

```clojure
#+BEGIN_QUERY
{:title
 [[:strong "⏰ Today"] [:span " or "] [:span.block-marker.NOW "NOW"]]
 :query [:find (pull ?block [*])
         :in $ ?day
         :where
         [?block :block/marker ?m]
         (or-join [?block ?day ?m]
                  (and
                   [(contains? #{"LATER"} ?m)]
                   (or [?block :block/scheduled ?d] [?block :block/deadline ?d])
                   [(= ?d ?day)])
                  ;;
                  (and
                   [(contains? #{"NOW"} ?m)]
                   [(missing? $ ?block :block/scheduled)]
                   [(missing? $ ?block :block/deadline)]))]

 :inputs [:today]
 :breadcrumb-show? false
 :collapsed? false}
#+END_QUERY
```

**What should I do tomorrow?** `LATER` tasks dated tomorrow:

```clojure
#+BEGIN_QUERY
{:title
 [:strong "🌞 Tomorrow"]
 :query [:find (pull ?block [*])
         :in $ ?day
         :where
         [?block :block/marker ?m]
         [(contains? #{"LATER"} ?m)]
         (or [?block :block/scheduled ?d] [?block :block/deadline ?d])
         [(= ?d ?day)]]
 :inputs [:1d-after]
 :breadcrumb-show? false
 :collapsed? false}
#+END_QUERY
```

**What can I do next?** `LATER` tasks without a date, and anything dated within the next week:

```clojure
#+BEGIN_QUERY
{:title
 [[:strong "📅 This week"] [:span " or "] [:span.block-marker.LATER "LATER"] [:sup "(without-date)"]]
 :query [:find (pull ?block [*])
         :in $ ?start ?next
         :where
         (or-join [?block ?start ?next]
                  (and
                   (or [?block :block/scheduled ?d] [?block :block/deadline ?d])
                   [(> ?d ?start)]
                   [(< ?d ?next)])
                  ;;
                  (and
                   [?block :block/marker ?m]
                   [(contains? #{"LATER"} ?m)]
                   [(missing? $ ?block :block/scheduled)]
                   [(missing? $ ?block :block/deadline)]))]

 :inputs [:1d-after :7d-after]
 :breadcrumb-show? false
 :collapsed? false}
#+END_QUERY
```

**What is waiting?** `WAITING` tasks, collapsed so they stay out of the way:

```clojure
#+BEGIN_QUERY

{:title
 [:strong "⏳ Waiting"]
 :query [:find (pull ?block [*])
         :where
         [?block :block/marker ?marker]
         [(contains? #{"WAITING"} ?marker)]]
 :breadcrumb-show? false
 :collapsed? true}
#+END_QUERY
```

## Pages and blocks

### Orphan pages

Lists the pages that no block links to:[^orphans]

```clojure
#+BEGIN_QUERY
{:title "orphan pages"
 :query [:find ?name
         :where
         [?p :page/name ?name]
         (not
          [?b :block/ref-pages ?p1]
          [?b :block/page ?p2]
          (or [?p1 :page/name ?name]
              [?p2 :page/name ?name]))]
 :view (fn [result]
         [:div.flex.flex-col
          (for [page result]
            [:a {:href (str "#/page/" page)} (clojure.string/capitalize page)])])}
#+END_QUERY
```

### Pages for PDF files

The page Logseq creates for a PDF has the properties `:file` and `:file-path`, so it can be found
with `has-page-property`:[^pdf]

```clojure
#+BEGIN_QUERY
{:query [:find (pull ?p [*])
         :where
         [has-page-property ?p :file]]}
#+END_QUERY
```

### The second block of a page

Finds the block that follows a page's properties block:[^second]

```clojure
#+BEGIN_QUERY
{:query [:find (pull ?bl [*])
         :where
         [?b :block/pre-block?]
         [?bl :block/left ?b]]}
#+END_QUERY
```

## Content

### Quotes

Logseq writes a quote either as a `#+BEGIN_QUOTE` block or as a line starting with `> `; this
query matches both:

```clojure
#+BEGIN_QUERY
 {:title "QUOTE search"
  :query [:find (pull ?b [*])
          :where
          [?b :block/content ?c]
          (or [(clojure.string/includes? ?c "#+BEGIN_QUOTE")]
              [(clojure.string/starts-with? ?c "> ")])]}
#+END_QUERY
```

### Highlighted text

Finds blocks with text highlighted by `==`. The first version matches any `==`:

```clojure
#+BEGIN_QUERY
 {:title "Highlight search"
  :query [:find (pull ?b [*])
          :where
          [?b :block/content ?c]
          [(clojure.string/includes? ?c "==")]]}
#+END_QUERY
```

The second needs a pair of `==` with text between them:

```clojure
#+BEGIN_QUERY
 {:title "Highlight search"
  :query [:find (pull ?b [*])
          :where
          [?b :block/content ?c]
          [(re-pattern "==.*==") ?regex]
          [(re-find ?regex ?c)]]}
#+END_QUERY
```

## Titles and templates

### HTML in a query title

`:title` accepts Hiccup as well as a string, as the dashboard above shows; the Hiccup references
in the reading list cover the syntax.[^title][^title-2]

### A query in a template

Template placeholders such as `<%current page%>` work inside a query, so a page template can carry
a query titled with the page's name that lists the blocks referencing it:[^template]

```clojure
#+BEGIN_QUERY
 {:title [:b "<%current page%>"]
  :query [:find (pull ?b [*])
          :in $ ?current-page
          :where
          [?b :block/path-refs ?name]
          [?name :block/name ?current-page]]
  :inputs [:current-page]
  :breadcrumb-show? false}
#+END_QUERY
```

[^rules]: [logseq/logseq: rules.cljc (L61–L141)](https://github.com/logseq/logseq/blob/2e340ca1c6d73731275d526c09e94d8d9e44214e/src/main/frontend/db/rules.cljc#L61-L141)
[^rules-example]: [logseq/logseq: query_custom_test.cljs (L26–L29)](https://github.com/logseq/logseq/blob/ed6a02c6921f6605d6c2482ca013f81de9c16b01/src/test/frontend/db/query_custom_test.cljs#L26-L29)
[^rules-gist]: [Examples of rules that can be used from advanced query](https://gist.github.com/logseq-cldwalker/796202d55897fd34ea3d8c3bb3401ae0)
[^docs-advanced]: [Logseq Docs: Advanced Queries](https://docs.logseq.com/#/page/advanced%20queries)
[^lowercase]: [`:current-page` and `<% current page %>` should not lower-case the page's title - comment](https://github.com/logseq/logseq/issues/4206#issuecomment-1038279559)
[^icon]: [Logseq Discord #workflows](https://discord.com/channels/725182569297215569/766475028978991104/961743401152286770)
[^number]: [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/966146506392473640)
[^release]: [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/963473363043512390)
[^journal-link]: [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/963685777122930698)
[^journal-link-2]: [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/963722478973243404)
[^namespace]: [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/962006699223429260)
[^week]: [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/955436664048717855)
[^recurring]: [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/966350378172035072)
[^today]: [Logseq Discord #general](https://discord.com/channels/725182569297215569/725182570131751005/952597973894848613)
[^orphans]: [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/832512082289229824)
[^pdf]: [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/957340667292577802)
[^second]: [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/966502226589777920)
[^title]: [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/955453273077338202)
[^title-2]: [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/955453575310499840)
[^template]: [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/960623468347527208)

