# In Progress
(In Progress, Ongoing)
```base
views:
  - type: table
    name: Table
    filters:
      and:
        - file.inFolder("Projects")
        - status.containsAny("In Progress", "Ongoing")
    order:
      - file.name
      - status
      - field
    sort:
      - property: status
        direction: ASC
      - property: file.name
        direction: ASC
    columnSize:
      file.name: 372
      note.status: 157

```

# Other Active
(Draft, Waiting For)
```base
views:
  - type: table
    name: Table
    filters:
      and:
        - file.inFolder("Projects")
        - status.containsAny("Draft", "Waiting For")
    order:
      - file.name
      - status
      - field
    sort:
      - property: status
        direction: ASC
      - property: file.name
        direction: ASC
    columnSize:
      file.name: 385
      note.status: 147

```

# Not In Work
(Cancelled, Finished)
```base
views:
  - type: table
    name: Table
    filters:
      and:
        - file.inFolder("Projects")
        - status.containsAny("Cancelled", "Finished")
    order:
      - file.name
      - status
      - field
    sort:
      - property: status
        direction: ASC
      - property: file.name
        direction: ASC
    columnSize:
      file.name: 394
      note.status: 157

```