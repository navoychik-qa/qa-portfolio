# API-тестирование панели управления Telegram-ботом

## 📋 Краткое описание

Тестирование REST API панели `panel.kabanya.ru` через Postman. 
Проверка авторизации, прав доступа, получения данных и обработки ошибок.

| Поле | Значение |
|------|----------|
| **Роль** | QA Engineer (практика) |
| **Инструменты** | Postman, Chrome DevTools |
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

📎 [Скачать коллекцию](./kabanya-panel-api.postman_collection.json)

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

**Запрос:** `GET https://panel.kabanya.ru/api/profile`  
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
