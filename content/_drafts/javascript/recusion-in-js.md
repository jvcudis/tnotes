---
title: "Recusion in Js"
date: 2018-09-23T21:00:14+12:00
draft: true
Categories: ["javascript"]
---

<state when i started learning recursion, java>
I hated recursion when I was .

<how i discovered recursion for JS>

<how it works...>

<the code...>
```
/*
  This recursive function injects total rows for expanded section, category
  and extcode rows, when specified in "keysAndTypes". Eg:

  keysAndTypes = [
    ['categories', ROW_TYPES.SECTION_TOTAL],
    ['extcodes', ROW_TYPES.CATEGORY_TOTAL],
    ['worksheetItems', ROW_TYPES.EXTCODE_TOTAL]
  ]
*/
export const injectTotalRows = (graph, keysAndTypes) => {
  return graph.reduce(
    (graph, row) => {
      if (isSection(row) || isCategory(row) || isExtcode(row)) {
        const [ [key, type], ...otherKeys ] = keysAndTypes
        const isExpanded = row.get(key)

        graph = graph.push(
          isExpanded && otherKeys.length
            ? row.set(key, injectTotalRows(row.get(key), otherKeys))
            : row
        )

        if (isExpanded) {
          const totalRow = row.set('type', type).set(key, undefined)
          graph = graph.push(totalRow)
        }
      } else {
        graph = graph.push(row)
      }

      return graph
    },
    List()
  )
}
```

<final thoughts?>
