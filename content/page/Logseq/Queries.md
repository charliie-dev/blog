---
title: Logseq | Queries
description: "Notes on Logseq advanced queries, from the background (Datalog, Datascript, Datomic, Logseq's database schema and Hiccup) to working examples: querying multiple tags, sorting by a block property, capturing quotes, scheduled and deadline tasks on today's journal, the current date, orphaned pages, queries inside templates, recurring tasks and more. Collected and tested from the Logseq Discord."
tags:
  - logseq
date: 2022-04-08
lastMod: 2022-04-21
---

from @Jeddychan in Logseq's discord #.v-border-children

- Terms / Names -  
  Datalog ------ a query language used for databases
  Datascript --- a flavor of Datalog written in Clojure. Logseq currently uses Datascript.
  Datomic ----- a different flavor of Datalog written in Clojure. (Some tutorials for Datomic may be helpful, but ymmv)
  Hiccup ------- a language used to generate HTML, you can use it in an advanced query's custom view ( :view )

- Official Docs -  
  [Logseq Docs: Queries](https://logseq.github.io/#/page/queries)
  [Logseq Docs: Advanced Queries](https://logseq.github.io/#/page/advanced%20queries)
  [Logseq Docs: Hiccup](https://logseq.github.io/#/page/hiccup)

- Datalog -  
  [Learn Datalog Today!](http://www.learndatalogtoday.org/)
  [Learn Datalog Today! - Extensible Data Notation](http://www.learndatalogtoday.org/chapter/0)
  When reading through this, I realized that viewing some of the query keywords as relationships between objects, made everything make a lot more sense/intuitive

- Datascript -  
  [DataScript Wiki: Getting started](https://github.com/tonsky/datascript/wiki/Getting-started)
  [tonsky/datascript - Resources](https://github.com/tonsky/datascript#resources)

- Datomic -  
  [Query Reference - Syntax Used In Grammar](https://docs.datomic.com/on-prem/query/query.html#syntax-used-in-grammar)
  Only some info here may be applicable, but I found the syntax section to be helpful in understanding the meaning of all the brackets used in queries (like: [ ], { }, ( ), etc.)

- Logseq's Database Schema -  
  [logseq/logseq: src/main/frontend/db_schema.cljs](https://github.com/logseq/logseq/blob/master/src/main/frontend/db_schema.cljs)
  Here you can see how the database is structured and all the keywords (you may recognize some keywords from example queries)

- Hiccup Syntax -  
  [Hiccup Wiki: Syntax](https://github.com/weavejester/hiccup/wiki/Syntax)

- Unofficial Docs -  
  [Logseq MSK Docs: Queries](https://mschmidtkorth.github.io/logseq-msk-docs/#/page/queries)

from @Bad3r in Logseq's discord #.v-border-children

- Datalog is what's called a rule based query language. It's been around since the 80s
  Some resources for learning Datalog.

- [Learn Datalog Today!](http://www.learndatalogtoday.org/)

- [Learn Crux Datalog Today](https://nextjournal.com/try/learn-crux-datalog-today/learn-crux-datalog-today)

- [Datomic Query](https://docs.datomic.com/query.html)

Logseq queries

- [Logseq MSK Docs: Queries/Advanced Queries/Tutorial](https://mschmidtkorth.github.io/logseq-msk-docs/#/page/Queries%2FAdvanced%20Queries%2FTutorial)

- [Logseq Docs: Advanced Queries](https://docs.logseq.com/#/page/advanced%20queries)

from @Avijeet in Logseq's discord #.v-border-children

- Hi, I have compiled a list of advanced queries and references from this channel, which I was able to execute for my database (still need to add those from the last few weeks). Thought it might be useful to those looking for advanced queries examples. Link to the original discord message and dates are also included for context 🙂.

- [[Logseq/Advanced_queries_Examples]]

from @cldwalker in Logseq's discord #.v-border-children

- There is a handy new query feature in the nightly build - the ability to use any of these simple query rules, [logseq/logseq: rules.cljc (L61–L141)](https://github.com/logseq/logseq/blob/2e340ca1c6d73731275d526c09e94d8d9e44214e/src/main/frontend/db/rules.cljc#L61-L141),

- in an advanced query. Here is an example query - [logseq/logseq: query_custom_test.cljs (L26–L29)](https://github.com/logseq/logseq/blob/ed6a02c6921f6605d6c2482ca013f81de9c16b01/src/test/frontend/db/query_custom_test.cljs#L26-L29) .

- While the rules have the same names as the operators in simple queries, their arguments can be different. Examples of their arguments are at [Examples of rules that can be used from advanced query](https://gist.github.com/logseq-cldwalker/796202d55897fd34ea3d8c3bb3401ae0)

- `:keys` logic for query:
  [DataScript queries - Return maps](https://github.com/tonsky/datascript/blob/master/docs/queries.md#return-maps)

Get all block's uuid

```Clojure
#+BEGIN_QUERY
{:title "tmp"
 :view (fn [result] (for [r result] [:pre (pr-str r)]))
 :query [:find (pull ?b [*])
       :where
       [?b :block/uuid _]]}
 :view (fn [result] (for [r result] [:pre (pr-str r)]))
#+END_QUERY
```

---

Example correction `6. All pages have a "programming" tag` in [Logseq Docs: Advanced Queries](https://docs.logseq.com/#/page/advanced%20queries)

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

---

Query multiple tags

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
           [?ref :block/name "othertag"])

       ]
 :inputs [:7d-before :today "TAG1"]}
#+END_QUERY
```

---

Sorting based on a block property

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

---

Display today page through query dynamically based on today's date

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
            [(>= ?d ?today)]

        ]
        :inputs [:today]
        :breadcrumb-show? false
        :result-transform (fn [result]
            (sort-by (fn [h]
            (get h :block/priority "Z")
            )
             result))
          :collapsed? false
}
#+END_QUERY
```

---

Capture all the quotes

- Logseq has two ways you can write quotes, you only match one, this should do the trick:

```clojure
#+BEGIN_QUERY
 {:title "QUOTE search"
  :query [:find (pull ?b [*])
  :where
   [?b :block/content ?c]
   (or [(clojure.string/includes? ?c "#+BEGIN_QUOTE")]
       [(clojure.string/starts-with? ?c "> ")]
   )]}
#+END_QUERY
```

---

Query for all the blocks which have a block property = current page, use the name of current page dynamically instead of fixing it on each page

- the page name have to be `lowercase`.

```clojure
#+BEGIN_QUERY
{:query [
 :find (pull ?b [*])
 :in $ ?current-page
 :where
  [?p :block/name ?current-page]
  [?b :block/page ?p]
  [?b :block/path-refs [:block/name "datalog"]]
  ]
 :inputs [:current-page]}
}
#+END_QUERY
```

- you can use `:block/name` or `:block/original-name` to differentiate, but the first is always lowercase (afaik), so it's not a good habit (on Logseq) to depend on differences in case.

```clojure
#+BEGIN_QUERY
{:query [:find (pull ?e [*])
:in $ ?current-page
:where
[?b :block/marker ?marker]
[(contains? #{"TODO" "LATER" "NOW" "DOING"} ?marker)]
[?e :block/properties ?prop]
[(get ?prop :projects) ?value]
[(contains? ?value ?current-page)]
]
:inputs [:current-page]
:result-transform (fn [result]

  (sort-by (fn [h]
      (get h :block/priority "Z")) result))
:collapsed? false}}
#+END_QUERY
```

- [`:current-page` and `<% current page %>` should not lower-case the page's title - comment](https://github.com/logseq/logseq/issues/4206#issuecomment-1038279559)

---

find block which are highlighted with ==

- sol. 1

```clojure
#+BEGIN_QUERY
 {:title "Highlight search"
  :query [:find (pull ?b [*])
  :where
   [?b :block/content ?c]
   [(clojure.string/includes? ?c "==")]
   ]}
#+END_QUERY
```

- sol. 2

```clojure
#+BEGIN_QUERY
 {:title "Highlight search"
  :query [:find (pull ?b [*])
  :where
   [?b :block/content ?c]
   [(re-pattern "{{< logseq/mark >}}.*{{< / logseq/mark >}}") ?regex]
   [(re-find ?regex ?c)]
   ]}
#+END_QUERY
```

---

Show scheduled or deadline only on today's journal

```clojure
#+BEGIN_QUERY
{:title "SCHEDULED OR DEADLINE"
 :query [
  :find (pull ?b [*])
  :in $ ?current-page
  :where
  [?p :block/name ?current-page]
  [?p :block/journal? true]
  [?p :block/journal-day ?date]
  (or
   [?b :block/scheduled ?date]
   [?b :block/deadline ?date]
  )
  (not [?b :block/marker])
 ]
 :inputs [:current-page]}
#+END_QUERY
```

---

Query the block & page title of certain pages or tags

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

---

Always query for current date / today

- [Logseq Discord #general](https://discord.com/channels/725182569297215569/725182570131751005/952597973894848613)

- `{{query <%today%>}}`

---

Concatenate all the journal entries of the week in the Friday entry.

- [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/955436664048717855)

```clojure
#+BEGIN_QUERY
{:title "What has been done"
 :query [:find (pull ?b [*])
         :in $ ?current-page ?start ?today
         :where
         [?b :block/page ?p]
         [?p :page/journal? true]
         [?p :page/journal-day ?d]
         ;[?p :block/name ?current-page]
         ;[?p :block/journal-day ?current-page-day]
         [(>= ?d ?start)]
         (or
          [(<= ?d ?current-page)]
          [(<= ?d ?today)])
         ]
 :inputs [:current-page :7d-before :today]}
#+END_QUERY
```

---

As I'm using NOW/LATER + WAITING approach, my common workflow for tasks life cycle is:

- add something not urgent (and visually "muted") with WAITING without date
- add common task with LATER + plan it if needed with deadline\scheduled
- add current\urgent task with NOW --> work on it --> finish with DONE
- take overdued tasks
- take some LATER task --> delete planed date if it was --> set it to NOW --> work --> finish with DONE

I was trying to mix 2 layers of tasks planning as you can see - task status cycle and "calendared" items:

- "what is overdued?"
- "what should I do today?"
- "what should I do tomorrow?"
- "what can I do next?"
- "what is WAITING?"

  - **PS**: no sorting in queries, as i want to see parent pages links 😒 #.v-gallery-h400-fit

- ✅ wanna have block answering my question "what is overdued?"
  - anything task calendared before today
  - also as this block can called "attention" - I'm searching for "wrong" tasks calendared NOW(it should no have date, as described before)

```clojure
#+BEGIN_QUERY
{
    :title
        [[:strong "🔥 Overdue"] [:span " or "] [:span.block-marker.NOW "NOW"] [:sup "(with-date)"]]
    :query [
        :find (pull ?block [*])
        :in $ ?start ?next
        :where
            [?block :block/marker ?m]
            (or-join [?block ?start ?next ?m]
                (and
                    (or [?block :block/scheduled ?d] [?block :block/deadline ?d])
                    [(> ?d ?start)]
                    [(< ?d ?next)]
                )
                ;;
                (and
                    [(contains? #{"NOW"} ?m)]
                    (or [?block :block/scheduled ?d] [?block :block/deadline ?d])
                )
            )
        ]
        :inputs [:365d-before :today]
        :breadcrumb-show? false
        :collapsed? false
}
#+END_QUERY
```

- ✅ wanna have block answering my question "what should i do today?"
  - not calendared status NOW = today
  - status LATER with date today

```clojure
#+BEGIN_QUERY
{
    :title
        [[:strong "⏰ Today"] [:span " or "] [:span.block-marker.NOW "NOW"]]
    :query [
        :find (pull ?block [*])
        :in $ ?day
        :where
            [?block :block/marker ?m]
            (or-join [?block ?day ?m]
                (and
                    [(contains? #{"LATER"} ?m)]
                    (or [?block :block/scheduled ?d] [?block :block/deadline ?d])
                    [(= ?d ?day)]
                )
                ;;
                (and
                    [(contains? #{"NOW"} ?m)]
                    [(missing? $ ?block :block/scheduled)]
                    [(missing? $ ?block :block/deadline)]
                )
            )
    ]
    :inputs [:today]
    :breadcrumb-show? false
    :collapsed? false
}
#+END_QUERY
```

- ✅ wanna have block answering my question "what should i do tomorrow?"
  - LATER with date tomorrow

```clojure
#+BEGIN_QUERY
{
    :title
        [:strong "🌞 Tomorrow"]
    :query [
        :find (pull ?block [*])
        :in $ ?day
        :where
            [?block :block/marker ?m]
            [(contains? #{"LATER"} ?m)]
            (or [?block :block/scheduled ?d] [?block :block/deadline ?d])
            [(= ?d ?day)]
    ]
    :inputs [:1d-after]
    :breadcrumb-show? false
    :collapsed? false
}
#+END_QUERY
```

- ✅ wanna have block answering my question "what can i do next?"
  - not calendared with status LATER = next
  - any status with date after tomorrow = next

```clojure
#+BEGIN_QUERY
{
    :title
        [[:strong "📅 This week"] [:span " or "] [:span.block-marker.LATER "LATER"] [:sup "(without-date)"]]
    :query [
        :find (pull ?block [*])
        :in $ ?start ?next
        :where
            (or-join [?block ?start ?next]
                (and
                    (or [?block :block/scheduled ?d] [?block :block/deadline ?d])
                    [(> ?d ?start)]
                    [(< ?d ?next)]
                )
                ;;
                (and
                    [?block :block/marker ?m]
                    [(contains? #{"LATER"} ?m)]
                    [(missing? $ ?block :block/scheduled)]
                    [(missing? $ ?block :block/deadline)]
                )
            )
    ]
    :inputs [:1d-after :7d-after]
    :breadcrumb-show? false
    :collapsed? false
}
#+END_QUERY
```

- ✅ wanna have block answering my question "what is WAITING?" (collapsed, to not blow my eyes)
  - not calendared with status WAITING

```clojure
#+BEGIN_QUERY

{
    :title
        [:strong "⏳ Waiting"]
    :query [
        :find (pull ?block [*])
        :where
            [?block :block/marker ?marker]
            [(contains? #{"WAITING"} ?marker)]
    ]
    :breadcrumb-show? false
    :collapsed? true
}
#+END_QUERY
```

---

Add HTML markup to query title

- [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/955453273077338202)

- [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/955453575310499840)

- [Hiccup Wiki: Syntax](https://github.com/weavejester/hiccup/wiki/Syntax)

- [yokolet/hiccup-samples](https://github.com/yokolet/hiccup-samples)

---

page for pdf files has property `:file` and `:file-path`:

- [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/957340667292577802)

```clojure
#+BEGIN_QUERY
{
 :query [:find (pull ?p [*])
         :where
         [has-page-property ?p :file]
       ]
}
#+END_QUERY
```

---

Find all unlinked pages/orphaned nodes

- [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/832512082289229824)

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

---

Use query in a template.

- [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/960623468347527208**)

```clojure
#+BEGIN_QUERY
 {:title [:b "<%current page%>"]
  :query [:find (pull ?b [*])
  :in $ ?current-page
  :where
    [?b :block/path-refs ?name]
    [?name :block/name ?current-page]
    ]
  :inputs [:current-page]
  :breadcrumb-show? false
 }
#+END_QUERY
```

---

Query for **icon page property**.

- [Logseq Discord #workflows](https://discord.com/channels/725182569297215569/766475028978991104/961743401152286770)

```clojure
#+BEGIN_QUERY
 {:title ""
 :query [:find (pull ?p [*])
 :where
    [?p :block/properties ?prop]
    [(get ?prop :icon) ?icon]
     ; [(= "example" ?type)]`
     ]}
#+END_QUERY
```

---

Get scheduled tasks within a namespace.

- [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/962006699223429260)

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

- `:block/namespace` are page property and `:block/name` are lowercase version of `:block/original-name`(page-name)

---

Filter the page with time/timestamp.

- [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/963473363043512390)

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

- Query for timestamp

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
         [(>  ?d ?today)]
         ]
 :inputs [:right-now-ms]
}
#+END_QUERY
```

- Query for yyyyMMdd

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
         [(>  ?d ?today)]
         ]
 :inputs [:today]
}
#+END_QUERY
```

    +

---

Using journal-date format in page property and query it.

- [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/963685777122930698)

- [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/963722478973243404)

- just tested it with different journal-date format. replace `[?p :block/name ?n]` with `[?p :block/original-name ?n]`. date value are stored in property with `original-name`.

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

---

Have a `property` field that I only fill with numbers (floats). Is it possible to pull blocks where this field is within a range (say 20 < x < 50, or just x > 100).

- [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/966146506392473640)

```clojure
#+BEGIN_QUERY
{
 :query [:find (pull ?b [*])
         :where
         [?b :block/properties ?prop]
         [(get ?prop :num-prop) ?num]
         [(> ?num 100)]
       ]
}
#+END_QUERY
```

- for 20<x<50 replace `[(> ?num 100)]` with

```clojure
         [(< ?num 50)]
         [(> ?num 20)]
```

- float function doesn't recognized, so multiply property by 1.0

```clojure
#+BEGIN_QUERY
{

 :query [:find (pull ?b [*])
         :where
         [?b :block/properties ?prop]
         [(get ?prop :num-prop) ?num]
         [(* 1.0 ?num) ?numf]
         [(< ?numf 50)]
         [(> ?numf 20)]
       ]
}#+END_QUERY
```

- ***

Query tasks which are recurring.

- [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/966350378172035072)

```clojure
#+BEGIN_QUERY
{
 :query [:find (pull ?b [*])
         :where
         [?b :block/repeated?]
       ]
}
#+END_QUERY
```

---

Query to get second block.

- [Logseq Discord #queries](https://discord.com/channels/725182569297215569/743139225746145311/966502226589777920)

```clojure
#+BEGIN_QUERY
{
 :query [:find (pull ?bl [*])
         :where
         [?b :block/pre-block?]
         [?bl :block/left ?b]
       ]
}
#+END_QUERY
```

---
