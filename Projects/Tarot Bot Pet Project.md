---
status: In Progress
start-date:
end-date:
related-projects:
field:
  - Career
  - Creativity
---
# Goal

Сделать телеграм-бота для чтения раскладов таро с AI-интерпретацией.

# Steps

## Подготовка

- [x] Определить технический стек
- [x] Выбрать колоду для MVP (Rider-Waite-Smith)
- [ ] Спроектировать архитектуру

## Разработка

- [ ] Базовый бот с меню
- [ ] Интеграция с AI для интерпретации
- [ ] Генерация визуализаций раскладов
- [ ] Telegram Mini App для истории

## Проверка качества

- [ ] Тестирование с реальными пользователями
- [ ] Проверка качества AI-ответов

## Запуск

- [ ] Деплой
- [ ] Настройка платежей

# Info

## Технический стек

> [!code]- Stack
> | Компонент | Технология |
> |-----------|------------|
> | Backend | Java + Spring Boot + TelegramBots |
> | AI | OpenAI API (GPT-4o-mini/nano) |
> | База данных | PostgreSQL |
> | Frontend | React (Telegram Mini Apps) |
> | Хостинг | Dokploy + Cloudflare Tunnel |
> | Визуализация | Java2D + ImageIO |

## Структура модулей

> [!code]- Пакеты
> ```
> com.tarotbot/
>   ├── bot/
>   │   ├── TarotBotApplication
>   │   ├── handlers/ (команды, callback, messages)
>   │   └── keyboard/ (меню, кнопки)
>   ├── api/
>   │   ├── UserController
>   │   ├── ReadingController
>   │   ├── DeckController
>   │   └── PaymentController
>   ├── service/
>   │   ├── UserService
>   │   ├── ReadingService
>   │   ├── SpreadService
>   │   ├── PaymentService
>   │   └── ImageGenerationService
>   ├── repository/    # JPA репозитории
>   ├── model/         # Сущности, DTO, enums
>   ├── integration/
>   │   ├── OpenAiClient (GPT + Whisper)
>   │   └── PaymentClient
>   └── util/          # Хелперы
> ```

## API эндпоинты

> [!code]- REST API для Mini App
> ### UserController
> - `GET /api/users/me` — профиль пользователя
> - `PATCH /api/users/me` — обновить настройки
>
> ### ReadingController
> - `GET /api/readings` — история раскладов (пагинация, фильтры)
> - `GET /api/readings/{id}` — детали расклада
> - `PATCH /api/readings/{id}` — обновить (избранное, заметки, архив)
>
> ### DeckController
> - `GET /api/decks` — список колод
> - `GET /api/decks/{id}/cards` — карты колоды
> - `GET /api/spread-types` — типы раскладов
>
> ### SkinController
> - `GET /api/skins` — каталог скинов
> - `GET /api/skins/{id}` — детали скина

## Схема данных

### Описание сущностей

> [!info]- Глоссарий
> **User** — пользователь бота, его настройки и баланс раскладов
>
> **Deck** — структура колоды (какие карты, их значения, порядок). Примеры: классическое Таро Уайта, Таро только на старших арканах, Ленорман, игральные карты
>
> **Card** — карта в колоде с названием и значениями (прямое/перевёрнутое)
>
> **Skin** — визуальное оформление для конкретной колоды. Один Deck может иметь много Skins (классический, минималистичный, арт-деко...)
>
> **SpreadType** — тип расклада (Кельтский крест, три карты...) с позициями и их значениями
>
> **Reading** — конкретный расклад пользователя: вопрос, карты, интерпретация
>
> **Product** — товар для покупки (пак раскладов, скин, подписка)
>
> **Payment** — транзакция покупки

> [!code]- User
> - id
> - telegram_id
> - username
> - display_name
> - gender
> - available_readings
> - language
> - preferred_deck_id
> - interpretation_style
> - spread_theme
> - owned_skins (JSONB)
> - last_active_at
> - is_blocked
> - created_at

> [!code]- Deck
> - id
> - name
> - description
> - deck_type (enum: STANDARD_78, MAJOR_ONLY_22, ORACLE, LENORMAND, PLAYING_CARDS)
> - cards_count
> - is_active

> [!code]- Card
> - id
> - deck_id
> - card_index
> - name
> - arcana (enum: MAJOR, MINOR, nullable)
> - suit (enum: WANDS, CUPS, SWORDS, PENTACLES, nullable)
> - meaning_upright
> - meaning_reversed

> [!code]- Skin
> - id
> - name
> - deck_id
> - author
> - preview_image
> - description
> - tags (JSONB)
> - is_default
> - is_active

> [!code]- SpreadType
> - id
> - name
> - description
> - category (enum: GENERAL, LOVE, CAREER, DECISION, SELF)
> - complexity (enum: SIMPLE, MEDIUM, COMPLEX)
> - reversed_mode (enum: UPRIGHT_ONLY, REVERSED_ONLY, MIXED)
> - layout (JSONB)
> - icon
> - sort_order
> - is_active

> [!code]- Reading
> - id
> - user_id
> - spread_type_id
> - question_text
> - question_type (enum: TEXT, VOICE)
> - is_photo_reading
> - reading_data (JSONB: deck_id, cards)
> - interpretation_text
> - interpretation_style
> - is_favorite
> - is_archived
> - user_notes
> - created_at

> [!code]- Product
> - id
> - name
> - type (enum: READINGS_PACK, SKIN, SUBSCRIPTION)
> - params (JSONB)
> - price
> - currency
> - is_active

> [!code]- Payment
> - id
> - user_id
> - product_id
> - amount
> - currency
> - payment_provider
> - provider_payment_id
> - status (enum: PENDING, COMPLETED, FAILED, REFUNDED)
> - created_at

## Use Cases

> [!example]- Сценарии использования
> 1. **Онбординг** — Пользователь подписывается на бота, тот представляется
> 2. **Главное меню** — Выбор из опций: расклад, уроки, настройки, оплата
> 3. **Выбор расклада** — Выбор из разных типов раскладов
> 4. **Ввод вопроса** — Ввод вопроса текстом или голосовым сообщением
> 5. **Анализ фото** — Загрузка фото расклада для анализа
> 6. **Генерация расклада** — Получение красивой визуализации расклада
> 7. **AI интерпретация** — Персонализированный AI ответ в разных стилях
> 8. **Справочник** — Информация о картах и типах раскладов
> 9. **Настройки** — Колода, стиль интерпретации, язык, тема
> 10. **Просмотр колоды** — Визуальное представление колоды
> 11. **Монетизация** — 3 бесплатных расклада, покупка паками
> 12. **Экспорт истории** — Экспорт раскладов в Mini App
> 13. **Каталог скинов** — Просмотр и покупка скинов для колод
> 14. **Избранные расклады** — Добавление в закладки, фильтр по избранному
> 15. **Заметки к раскладу** — Личные заметки пользователя к раскладам

## Идеи

- **Реверс-расклад** — пользователь описывает желаемое значение, AI подбирает подходящие карты
- **Рейтинг раскладов** — пользователь оценивает расклад (1-5), для аналитики качества и улучшения AI
- **Ачивменты** — геймификация: "Первый расклад", "10 раскладов", "Все Major Arcana" и т.д.
- **Другие форматы ответов** — mp3, видео, web app
- **Другие фронтенды** — соц-сети, онлайн-магазин, веб-сайт
- **Экспорт истории в файл** — PDF, JSON экспорт раскладов
