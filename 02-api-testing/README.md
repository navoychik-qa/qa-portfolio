# API-тестирование панели управления Telegram-ботом

## 📋 Краткое описание

Тестирование REST API панели `panel.kabanya.ru` через Postman. 
Проверка авторизации, прав доступа, получения данных и обработки ошибок.

| Поле | Значение |
|------|----------|
| **Роль** | QA Engineer (практика) |
| **Инструменты** | Postman, Chrome / Opera DevTools |
| **Тип** | API-тестирование |
| **Дата** | Октябрь 2026 |
| **Проверено эндпоинтов** | 6 |

---

## 🎯 Что тестировал

- Авторизация через Cookie (`aurora_session`)
- Получение профиля (`/api/profile`)
- Получение информации о текущем пользователе и его правах (`/api/me`)
- Список забаненных ботов (`/api/banned_bots`)
- Обработка ошибок: 401, 404, 405
- Проверка прав доступа к административным эндпоинтам

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

### 1. GET `/api/profile` → 200 OK

Возвращает JSON с профилем пользователя: bio, display_name, email, job_title, notifications, security, theme, username.

### 2. GET `/api/me` → 200 OK

Возвращает JSON с ролью, правами и данными пользователя. 
Позволил обнаружить баг: `permissions.admin: false` при `role: "Administrator"`.

### 3. GET `/api/banned_bots` → 200 OK

Возвращает JSON со списком забаненных ботов.

---

## ❌ Негативные проверки

### 1. GET `/api/profile` без Cookie → 401 Unauthorized

Тело ответа пустое. Сервер корректно не пускает без авторизации.

### 2. PUT `/api/chat/list` → 405 Method Not Allowed

```json
{
  "reason": "method_not_allowed",
  "status": "error"
}

### 3. GET '/api/config/9999999' → 404 Not Found

```json
{
  "reason": "unknown_endpoint",
  "status": "error"
}

🐞 Найденные баги
Баг: 403 Forbidden при запросе /api/admin/users под ролью Administrator
Поле	Значение
Severity	Critical
Priority	ASAP
Статус	✅ Исправлен, верифицирован
Описание:
При отправке GET /api/admin/users с Cookie администратора сервер возвращал 403 Forbidden.
Через /api/me подтверждено: permissions.admin: false при роли Administrator.

Что сделал:

Воспроизвёл через Postman

Зафиксировал несоответствие прав через /api/me

Оформил баг-репорт

После фикса проверил: permissions.admin стал true, 403 исчез

📄 Полный баг-репорт

📎 Артефакты
Баг-репорты
BUG-API-001: 403 Forbidden при запросе /api/admin/users

Тест-кейсы
TC-001: Проверка авторизации через Cookie

TC-002: Проверка обработки неверного метода

TC-003: Проверка обработки несуществующего ID

Коллекция Postman
kabanya-panel-api.postman_collection.json

Скриншоты
Скриншоты API-тестирования

📊 Результат
Проверено 6 эндпоинтов (3 позитивных, 3 негативных)

Найден критичный баг (403), исправлен и верифицирован

Создана и экспортирована коллекция Postman

Получен опыт API-тестирования и работы с правами доступа

🛠️ Инструменты
Postman — отправка запросов, сохранение коллекции, работа с Cookie

DevTools — анализ реальных запросов, копирование Cookie

JSON — формат данных

HTTP — методы GET, PUT; статусы 200, 401, 403, 404, 405
