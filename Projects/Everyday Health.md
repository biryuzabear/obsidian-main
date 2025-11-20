---
status: Ongoing
start-date:
end-date:
related-projects:
field:
  - Food
  - Sport
  - Health
---
# Goal

Улучшить качество жизни и качество здоровья
# Steps

## Питание

- [ ] Составить список блюд которые можно есть в каждый приём пищи
- [ ] Регулярно закупать эти продукты
- [ ] Есть только их (организовать привычку)
## Спорт

- [ ] Составить список простых упражнений на каждый день
- [ ] Сформировать привычку
## Врачи

- [ ] Дантист (раз в полгода)
- [ ] Инфекционсит (раз в три месяца)
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
