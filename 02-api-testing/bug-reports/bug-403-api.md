# Баг: 403 Forbidden при запросе `/api/admin/users` под ролью Administrator

## 📋 Основная информация

| Поле | Значение |
|------|----------|
| **ID** | BUG-API-001 |
| **Type** | Баг |
| **Severity** | Critical |
| **Priority** | ASAP |
| **Статус** | ✅ Исправлен, верифицирован |
| **Связанный тест-кейс** | [TC-002 в 01-kabanya-panel](../../01-kabanya-panel/test-cases/tc-002-admin-data.md) |

---

## 📝 Description

При отправке `GET /api/admin/users` с Cookie администратора сервер возвращает 403 Forbidden. 
Через `GET /api/me` подтверждено несоответствие: `role: "Administrator"`, 
но `permissions.admin: false`. Из-за этого запросы к `/api/admin/*` недоступны, 
и админ-панель не работает корректно.

---

## 🌍 Environment

| Поле | Значение |
|------|----------|
| **Сайт** | https://panel.kabanya.ru |
| **Инструмент** | Postman |
| **Браузер** | Opera 135.0.5973.92 Stable |
| **ОС** | Windows 11 Pro 26200.9457 |
| **Роль** | Tester (Administrator) |

---

## ✅ Preconditions

1. Пользователь авторизован в браузере под ролью Administrator.
2. Cookie `aurora_session` скопирована в Postman.
3. Postman открыт для отправки запросов.

---

## 🔁 Steps to Reproduce (STR)

1. Открыть Postman.
2. Создать запрос `GET https://panel.kabanya.ru/api/admin/users`.
3. Добавить заголовок: `Cookie: aurora_session=...`
4. Нажать Send.
5. Повторить для `GET https://panel.kabanya.ru/api/admin/permissions`.
6. Дополнительно проверить `GET /api/me` — там видно `permissions.admin: false`.

---

## ❌ Actual Result

1. `GET /api/admin/users` далее **403 Forbidden**
2. `GET /api/admin/permissions` далее **403 Forbidden**
3. В ответе `/api/me`: `"role": "Administrator"`, но `"permissions.admin": false`

---

## ✅ Expected Result

1. `GET /api/admin/users` далее **200 OK** + JSON со списком пользователей
2. `GET /api/admin/permissions` далее **200 OK** + JSON с правами
3. В `/api/me`: `"permissions.admin": true` для роли Administrator

---

## 💬 Comments

**Разработчик (fix):**  
> Причина: в дефолтах прав Administrator имел `admin: false`. 
> Исправлено: `effective_permissions()` объединяет права с дефолтами, 
> держит инварианты: Developer — всегда всё, Administrator — всегда admin.

**Тестировщик (верификация):**  
> Проверил через Postman после фикса:
> - `/api/admin/users` → 200 OK
> - `/api/admin/permissions` → 200 OK
> - `/api/me` → `permissions.admin: true`
> - В UI панели: виджеты загружаются, фильтры работают
> 
> Баг подтверждён как исправленный.

---
