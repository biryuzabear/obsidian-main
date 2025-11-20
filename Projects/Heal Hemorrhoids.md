---
status: Waiting For
start-date:
end-date:
related-projects:
field:
  - Health
tags:
---
# Goal

Вылечить геморрой
# Steps

- [ ] Дождаться термина
# Info

## Лекарства

- Hametum (свечи и мазь, вроде более менее норм, но я бы посмотрл что ещё есть на рынке)

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
