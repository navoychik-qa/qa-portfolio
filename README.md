# Портфолио QA Engineer — Юрий.

## 👋 Обо мне

Начинающий QA Engineer. Прошёл курс по тестированию ПО, изучаю ручное тестирование, DevTools, API, Jira. 
Имею практический опыт тестирования реального проекта — панели управления Telegram-ботом для модерации чатов и каналов.

Ищу позицию **Junior QA Engineer**.

---

## 🛠️ Навыки

**Ручное тестирование:**
- Составление чек-листов и тест-кейсов
- Функциональное тестирование
- Тестирование UI/UX
- Тестирование прав доступа
- Регрессионное тестирование
- Верификация багов

**Инструменты:**
- DevTools (Network, Console, Elements)
- Jira / баг-трекеры
- Postman (базово)
- Git / GitHub

**Техники тест-дизайна:**
- Эквивалентное разбиение
- Граничные значения
- Таблица решений
- Диаграмма состояний

**Технологии:**
- HTTP, статусы (200, 403, 404, 500)
- REST API (GET, POST)
- SQL (базово)

---

## 📁 Кейсы

### 1. [Тестирование панели управления Telegram-ботом](./01-https://panel.kabanya.ru)
Практическое тестирование реального проекта `panel.kabanya.ru`. 
Найдено на данный момент 2 бага, оба исправлены разработчиком и верифицированы.

**Найденные баги:**
- 🔴 Critical: 403 Forbidden при загрузке административных данных
- 🟠 Major: Фильтр упоминаний ботов пропускает слитные упоминания

### 2. [API-тестирование](./02-api-testing/)

# API-тестирование панели управления Telegram-ботом

## 📋 Краткое описание

Тестирование REST API панели `panel.kabanya.ru` через Postman. 
Проверка авторизации, прав доступа, получения данных и обработки ошибок.

| Поле | Значение |
|------|----------|
| **Роль** | QA Engineer (практика) |
| **Инструменты** | Postman, Opera DevTools |
| **Тип** | API-тестирование |
| **Дата** | Октябрь 2026 |

---

## 🎯 Что тестировал

- Авторизация через Cookie (`aurora_session`)
- Получение профиля (`/api/profile`)
- Получение информации о текущем пользователе и его правах (`/api/me`)
- Список забаненных ботов (`/api/banned_bots`)
- Обработка ошибок: 401, 404, 405

---

## 📁 Коллекция Postman

📎 [Скачать коллекцию](./Kabanya%20Panel%20API.postman_collection.json)

### Проверенные эндпоинты

| Метод | URL | Описание | Статус |
|-------|-----|----------|--------|
| GET | `/api/profile` | Информация о профиле | 200 OK |
| GET | `/api/me` | Информация о пользователе и правах | 200 OK |
| GET | `/api/banned_bots` | Список забаненных ботов | 200 OK |
| GET | `/api/profile` (без Cookie) | Проверка авторизации | 401 Unauthorized |
| PUT | `/api/chat/list` | Проверка метода | 405 Method Not Allowed |
| GET | `/api/config/9999999` | Несуществующий ID | 404 Not Found |

---

## ✅ Позитивные проверки

### 1. GET `/api/profile`

**Запрос:**  
`GET https://panel.kabanya.ru/api/profile`  
**Заголовок:** `Cookie: aurora_session=...`

**Ответ:** `200 OK`
```json
{
  "profile": {
    "bio": "Ультралорд",
    "display_name": "Тестировщик",
    "email": "Tester1@aurora.local",
    "job_title": "Administrator",
    "notifications": {
      "email": false,
      "mentions": true,
      "project_updates": true,
      "push": true,
      "weekly_digest": false
    },
    "security": {
      "login_alerts": true,
      "two_factor": true
    },
    "theme": "snow",
    "username": "Tester1"
  },
  "telegram_id": "1154256660",
  "two_factor": true
}

---
Проверки:

Статус 200 OK

Content-Type: application/json

Все поля профиля на месте

telegram_id совпадает с ожидаемым

2. GET /api/me
Запрос: GET https://panel.kabanya.ru/api/me
Заголовок: Cookie: aurora_session=...

Ответ: 200 OK
{
  "active": true,
  "display_name": "Тестировщик",
  "id": 4,
  "name": "Тестировщик",
  "permissions": {
    "admin": false,
    "con_center": true,
    "desk": true,
    "index": true,
    "kbase": true,
    "lal": true,
    "lchat": true,
    "settings": true,
    "stat": false,
    "user": true
  },
  "role": "Administrator",
  "telegram_id": "1154256660",
  "two_factor": true,
  "username": "Tester1"
}

Проверки:

Статус 200 OK

role: Administrator — корректно

permissions.admin: false — НЕКОРРЕКТНО для роли Administrator (см. баг ниже)

3. GET /api/banned_bots
Запрос: GET https://panel.kabanya.ru/api/banned_bots
Заголовок: Cookie: aurora_session=...

Ответ: 200 OK

{
  "8139831939": {
    "user_id": 8139831939,
    "username": "neirohambot",
    "first_name": "Нейрохам",
    "banned_at": "2026-09-30 15:59:39",
    "reason": "Неавторизованный бот"
  }
}

Проверки:

Статус 200 OK

Список забаненных ботов отображается

Все поля присутствуют

❌ Негативные проверки
1. GET /api/profile без Cookie → 401
Запрос: GET https://panel.kabanya.ru/api/profile без заголовка Cookie

Ответ: 401 Unauthorized
Тело ответа пустое. Content-Type: text/plain.

Ожидание: 401 — корректно. Сервер не пускает без авторизации.

2. PUT /api/chat/list → 405
Запрос: PUT https://panel.kabanya.ru/api/chat/list
Заголовок: Cookie: aurora_session=...

Ответ: 405 Method Not Allowed

{
  "reason": "method_not_allowed",
  "status": "error"
}

Ожидание: 405 — корректно. Эндпоинт принимает только GET.

3. GET /api/config/9999999 → 404
Запрос: GET https://panel.kabanya.ru/api/config/9999999

Ответ: 404 Not Found

{
  "reason": "unknown_endpoint",
  "status": "error"
}
Ожидание: 404 — корректно. Несуществующий ID.

🐞 Найденные баги
Баг: permissions.admin = false при роли Administrator
Поле	Значение
Severity	Critical
Priority	ASAP
Статус	Исправлен, верифицирован
Описание:
При запросе GET /api/me с Cookie администратора сервер возвращает role: "Administrator", но permissions.admin: false. Из-за этого запросы к /api/admin/* возвращают 403 Forbidden, и админ-панель не работает корректно.

Что сделал:

Обнаружил несоответствие через Postman

Подтвердил, что /api/admin/users возвращает 403

Оформил баг-репорт

После фикса проверил: permissions.admin стал true, 403 исчез

📄 Полный баг-репорт

📊 Результат
Проверено 6 эндпоинтов (3 позитивных, 3 негативных)

Подтверждён критичный баг с правами доступа (403)

Получен опыт API-тестирования через Postman

Создана и экспортирована коллекция запросов

📎 Артефакты
Коллекция Postman

Скриншоты

Баг-репорты

🛠️ Инструменты
Postman — отправка запросов, сохранение коллекции

Chrome DevTools — анализ реальных запросов, копирование Cookie

JSON — формат данных

HTTP — методы GET, PUT; статусы 200, 401, 404, 405

## 📞 Контакты

- Email: hank1414@mail.ru
- Telegram: @TommyRailey
---

## 📌 Статус

- 🟢 Открыт к предложениям
- 📅 Доступен с [30.09.2026]
