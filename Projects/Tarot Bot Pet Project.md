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
- [x] Спроектировать архитектуру

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
> | Whisper | Локальный (whisper-asr-webservice) |
> | Миграции | Flyway |
> | Логирование | SLF4J + Logback + Spring Boot Admin |
> | Деплой | Dokploy из GitHub |

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
>   │   ├── OpenAiClient (GPT)
>   │   ├── WhisperClient (локальный)
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

## Системный дизайн

> [!info]- Решения
> ### Аутентификация
> - Проверка initData от Telegram (подпись + auth_date)
> - Без JWT, без ролей
>
> ### Обработка ошибок
> - Global Exception Handler
> - Единый формат ответа: `{error, message, details}`
> - RuntimeException для бизнес-ошибок
> - Swagger документация для каждого эндпоинта
>
> ### Кэширование
> - Registry при старте: Decks, Cards, SpreadTypes, Skins, Products
> - Caffeine cache: Users (evict при обновлении)
> - Без кэша: Readings, Payments
>
> ### Логирование
> - INFO в консоль
> - DEBUG в файл
> - Ротация ежедневно, хранить 30 дней
> - Spring Actuator health check
>
> ### Безопасность
> - Валидация в DTO (@Valid, @Size)
> - CORS только для Telegram доменов
> - Секреты в env переменных
> - Prompt injection защита в системном промпте
> - Лимит голосового 60 сек
> - Лимит вопроса 2000 символов

## Компоненты

> [!code]- Архитектура
> ```
> [Telegram] → webhook → [Bot Handlers]
> [Mini App] → REST → [API Controllers]
>                            ↓
>                      [Services] ←→ [Registry]
>                            ↓
>                      [Repository] → [PostgreSQL]
>                            ↓
>               [OpenAI Client] → [OpenAI API]
>               [Whisper Client] → [Whisper Container]
> ```

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
> - telegram_id — ID пользователя в Telegram
> - username — @username в Telegram
> - display_name — как обращаться к пользователю
> - gender — для правильного обращения (он/она/они)
> - available_readings — количество доступных раскладов (уменьшается при использовании)
> - language — язык интерфейса и интерпретаций
> - preferred_deck_id — выбранная колода по умолчанию
> - interpretation_style — стиль AI ответов (краткий/подробный/поэтичный)
> - spread_theme — визуальная тема для раскладов
> - owned_skins (JSONB) — массив ID купленных скинов `[1, 2, 5]`
> - last_active_at — для аналитики активности
> - is_blocked — бан за абуз
> - created_at

> [!code]- Deck
> - id
> - name — название колоды
> - description — описание и история колоды
> - deck_type (enum: STANDARD_78, MAJOR_ONLY_22, ORACLE, LENORMAND, PLAYING_CARDS) — тип определяет совместимость с раскладами
> - cards_count — количество карт в колоде
> - is_active — можно скрыть без удаления

> [!code]- Card
> - id
> - deck_id — к какой колоде принадлежит
> - card_index — порядковый номер (0-77 для стандартной колоды)
> - name — название карты
> - arcana (enum: MAJOR, MINOR, nullable) — null для нестандартных колод
> - suit (enum: WANDS, CUPS, SWORDS, PENTACLES, nullable) — масть, null для Major Arcana
> - meaning_upright — значение в прямом положении
> - meaning_reversed — значение в перевёрнутом положении

> [!code]- Skin
> - id
> - name — название скина
> - deck_id — для какой колоды (обязательно)
> - author — имя художника
> - preview_image — путь к превью
> - description — описание стиля
> - tags (JSONB) — массив тегов для фильтрации `["modern", "dark", "geometric"]`
> - is_default — скин по умолчанию для колоды
> - is_active — можно скрыть без удаления
>
> Изображения карт по конвенции: `decks/{deck_id}/skins/{skin_id}/{card_index}.png`

> [!code]- SpreadType
> - id
> - name — название расклада
> - description — описание и когда использовать
> - category (enum: GENERAL, LOVE, CAREER, DECISION, SELF) — для фильтрации по теме
> - complexity (enum: SIMPLE, MEDIUM, COMPLEX) — сложность расклада
> - reversed_mode (enum: UPRIGHT_ONLY, REVERSED_ONLY, MIXED) — разрешены ли перевёрнутые карты
> - layout (JSONB) — схема расположения карт (см. ниже)
> - icon — эмодзи для меню
> - sort_order — порядок в списке
> - is_active — можно скрыть без удаления
>
> **Структура layout (JSONB):**
> ```json
> {
>   "aspectRatio": "4:3",
>   "cardScale": 0.15,
>   "background": "default",
>   "positions": [
>     {
>       "index": 1,
>       "name": "Настоящее",
>       "description": "Текущая ситуация",
>       "x": 0.5,
>       "y": 0.5,
>       "rotation": 0
>     }
>   ]
> }
> ```
>
> **Как работает:**
> - `aspectRatio` — пропорции холста для генерации изображения
> - `cardScale` — размер карты относительно холста (0.0-1.0)
> - `x`, `y` — относительные координаты (0.0-1.0), масштабируются под любой размер
> - `rotation` — угол поворота карты в градусах
> - `description` — используется AI для интерпретации позиции

> [!code]- Reading
> - id
> - user_id — чей расклад
> - spread_type_id — тип расклада
> - question_text — вопрос пользователя (или транскрипция голосового)
> - question_type (enum: TEXT, VOICE) — для аналитики
> - is_photo_reading — расклад по фото (AI распознал карты)
> - reading_data (JSONB) — данные расклада (см. ниже)
> - interpretation_text — ответ AI
> - interpretation_style — стиль интерпретации (для аналитики)
> - is_favorite — в закладках
> - is_archived — скрыт из истории
> - user_notes — личные заметки пользователя
> - created_at
>
> **Структура reading_data (JSONB):**
> ```json
> {
>   "deck_id": 1,
>   "cards": [
>     {"position": 1, "card_index": 0, "reversed": false},
>     {"position": 2, "card_index": 22, "reversed": true}
>   ]
> }
> ```
>
> **Как работает:**
> - `deck_id` — какая колода использовалась
> - `position` — номер позиции в раскладе (соответствует index из layout)
> - `card_index` — номер карты в колоде
> - `reversed` — перевёрнута ли карта

> [!code]- Product
> - id
> - name — название товара
> - type (enum: READINGS_PACK, SKIN, SUBSCRIPTION) — тип товара
> - params (JSONB) — параметры в зависимости от типа (см. ниже)
> - price — цена
> - currency — валюта
> - is_active — можно скрыть без удаления
>
> **Структура params (JSONB) по типам:**
> ```json
> // READINGS_PACK
> {"readings_amount": 10}
>
> // SKIN
> {"skin_id": 5}
>
> // SUBSCRIPTION
> {"duration_days": 30, "features": ["unlimited_readings", "all_skins"]}
> ```

> [!code]- Payment
> - id
> - user_id — кто платит
> - product_id — что покупает
> - amount — сумма
> - currency — валюта
> - payment_provider — Telegram Stars, ЮKassa и т.д.
> - provider_payment_id — ID транзакции у провайдера
> - status (enum: PENDING, COMPLETED, FAILED, REFUNDED) — статус платежа
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

## Roadmap

> [!note]- Фазы разработки
> ### Фаза 1: Инфраструктура
> - Создать репозиторий
> - Настроить Spring Boot проект
> - Docker + PostgreSQL
> - Flyway миграции (все таблицы)
>
> ### Фаза 2: Модель данных
> - JPA сущности
> - Репозитории
> - Registry для справочников
> - Тестовые данные (колода Rider-Waite, типы раскладов)
>
> ### Фаза 3: Базовый бот
> - Регистрация бота
> - Webhook
> - Онбординг
> - Главное меню
>
> ### Фаза 4: Генерация раскладов
> - Логика генерации карт
> - Визуализация (Java2D)
> - Сохранение в БД
>
> ### Фаза 5: AI интерпретация
> - OpenAI интеграция
> - Whisper интеграция
> - Промпты для интерпретации
>
> ### Фаза 6: Mini App
> - React приложение
> - API эндпоинты
> - История раскладов
> - Избранное, заметки
>
> ### Фаза 7: Деплой
> - Dokploy настройка
> - Cloudflare Tunnel
> - Мониторинг
>
> ### Фаза 8: Монетизация
> - Telegram Stars / платёжка
> - Покупка паков

## Идеи

- **Реверс-расклад** — пользователь описывает желаемое значение, AI подбирает подходящие карты
- **Рейтинг раскладов** — пользователь оценивает расклад (1-5), для аналитики качества и улучшения AI
- **Ачивменты** — геймификация: "Первый расклад", "10 раскладов", "Все Major Arcana" и т.д.
- **Другие форматы ответов** — mp3, видео, web app
- **Другие фронтенды** — соц-сети, онлайн-магазин, веб-сайт
- **Экспорт истории в файл** — PDF, JSON экспорт раскладов
