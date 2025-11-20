---
status: Draft
start-date:
end-date:
related-projects:
field:
tags:
---
# Goal


# Steps


# Info


# Events

```base
views:
  - type: table
    name: Table
    filters:
      and:
        - file.inFolder("Events")
        - note["related-projects"].contains(this.file.asLink())
    order:
      - file.name
      - start-date
      - event-type
    sort:
      - property: start-date
        direction: DESC
    columnSize:
      note.event-type: 120
```
