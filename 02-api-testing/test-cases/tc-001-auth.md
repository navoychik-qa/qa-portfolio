# TC-001: Проверка авторизации через Cookie

## 📋 Основная информация

| Поле | Значение |
|------|----------|
| **ID** | TC-001 |
| **Название** | Проверка авторизации через Cookie `aurora_session` |
| **Приоритет** | High |
| **Тип** | API / Авторизация |
| **Компонент** | `/api/profile`, `/api/me` |
| **Статус** | ✅ Выполнен |
| **Инструмент** | Postman |

---

## ✅ Предусловия

| № | Условие |
|---|---------|
| 1 | Пользователь авторизован в браузере на `panel.kabanya.ru` |
| 2 | Cookie `aurora_session` скопирована из DevTools далее Application далее Cookies |
| 3 | Postman открыт, создан новый GET-запрос |

---

## 🔁 Шаги выполнения

| № | Действие | Ожидаемый результат | Фактический результат | Статус |
|---|----------|---------------------|------------------------|--------|
| 1 | `GET /api/profile` без заголовка Cookie | 401 Unauthorized | 401 Unauthorized | ✅ Passed |
| 2 | `GET /api/profile` с Cookie `aurora_session=...` | 200 OK, JSON с профилем | 200 OK | ✅ Passed |
| 3 | `GET /api/me` с Cookie `aurora_session=...` | 200 OK, JSON с ролью и правами | 200 OK | ✅ Passed |
| 4 | `GET /api/profile` с неверным Cookie | 401 Unauthorized | 401 Unauthorized | ✅ Passed |

---

## 📊 Итог

| Всего шагов | Passed | Failed |
|-------------|--------|--------|
| 4 | 4 | 0 |

**Вывод:** авторизация через Cookie работает корректно. Без Cookie — 401, с Cookie — 200 OK.

---

## 🐞 Связанные баги

| ID | Название | Severity | Статус |
|----|----------|----------|--------|
| — | — | — | — |

---

## 📎 Вложения

<img width="1053" height="764" alt="image" src="https://github.com/user-attachments/assets/4452dfd3-0bf3-47f1-a1f0-e3d0fd8624cf" />

<img width="1058" height="777" alt="image" src="https://github.com/user-attachments/assets/73a6eac1-4486-4ad3-8956-3a71724b92ad" />
