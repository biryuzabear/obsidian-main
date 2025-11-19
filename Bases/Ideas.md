# Lists

```base
views:
  - type: table
    name: Table
    filters:
      and:
        - file.inFolder("Ideas/Lists")
    order:
      - file.name

```

# New Ideas

```base
views:
  - type: table
    name: Table
    filters:
      and:
        - file.inFolder("Ideas")
        - file.hasProperty("field")
        - note["have-tried"] == false
    order:
      - file.name
      - field
      - have-tried
      - tags
    sort:
      - property: file.name
        direction: ASC
    columnSize:
      file.name: 191
      note.field: 146

```

# Have Tried

```base
views:
  - type: table
    name: Table
    filters:
      and:
        - file.inFolder("Ideas")
        - file.hasProperty("field")
        - note["have-tried"] == true
    order:
      - file.name
      - field
      - tags
    sort:
      - property: field
        direction: ASC
    columnSize:
      file.name: 191
      note.field: 154

```
