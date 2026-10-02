# Дорожная карта обучения: от экзамена Эльбруса до пет-проекта для собеседований

База для повторения: экзаменационный проект `Abdrey05/Sova` (React+TS+Vite+FSD / Express+Sequelize+Postgres / JWT).
Новый проект пишем **с нуля, своими руками**. Я объясняю, даю документацию и делаю code review. Ты пишешь код сам.

---

## Какой проект строим и почему

**«Slotly» — онлайн-запись для сферы услуг** (барбершоп, салон красоты, репетитор, стоматолог):
клиент видит расписание и записывается, мастер видит свои записи, админ управляет услугами и расписанием.

Почему именно он:

1. **Собеседования.** Проект закрывает чек-лист уверенного джуна+: REST API, реляционная БД и связи,
   авторизация с ролями, JWT + refresh, TypeScript, тесты, Docker/CI, деплой, README.
   Каждый пункт можно показать кодом и объяснить словами.
2. **Бизнес.** Это мини-аналог YClients/Dikidi. Маленьким салонам такое продают по 500–2000 ₽/мес.
   Реальная боль: записи в WhatsApp/тетради теряются, напоминания клиентам не уходят.
   Значит проект можно показать не только как «пет-проект», а как продукт с демо и питчем.
3. **Педагогика.** Идеально дозируется по сложности: от «массив в памяти» до транзакций и пересечений интервалов.
4. **Связь со старым проектом.** На этапах авторизации и FSD мы будем открывать `Sova` и сравнивать:
   «вот как мы делали с преподавателем, вот почему так, вот как можно лучше». Это и есть закрепление.

Имя рабочее — можешь переименовать во что угодно, суть не меняется.

---

## Как устроены наши уроки (цикл этапа)

1. **Теория по документации.** Я даю ссылки на официальные docs + 3 вопроса («документационное задание»),
   на которые можно ответить только прочитав. Отвечаешь своими словами.
2. **Практика.** Ты пишешь код этапа у себя, пушишь в GitHub, кидаешь ссылку/коммит.
3. **Review.** Я читаю код, разбираю ошибки, показываю «как сделал бы я и почему».
4. **Ретро в журнале.** 5 строк в `learning/JOURNAL.md`: что сделал, что понял, что застряло, что почитаю ещё.

**Правило 10 минут:** застрял → прочитай сообщение об ошибке вслух себе → поищи в официальной документации
→ только потом пиши мне, приложив: текст ошибки + что уже прочитал + что попробовал.
Это и есть навык работы с docs, он качается только так.

**Анатомия документации (выучить наизусть):**
- *Guide / Tutorial* — учит концепции, читается подряд (Express Guide, react.dev Learn).
- *API Reference* — справочник функций/опций, читается выборочно через Ctrl+F (sequelize.org API).
- Всегда смотри: версию docs, раздел Getting Started, примеры кода, раздел Migration/Changelog при обновлениях.
- MDN (developer.mozilla.org) — первоисточник по JS/HTTP/браузеру. Привыкай гуглить запросами вида `mdn fetch`, `mdn cookies`.

Темп: 1 этап ≈ 1 неделя при 6–10 часах. Скажешь свой реальный темп — перекрою план.

---

## Этапы

### Этап 0. Фундамент и правила игры (повторение базы)
- Новый репозиторий, структура `client/` + `server/`, `.gitignore`, `.env.example`, README с описанием продукта.
- Ветвление: `main` + ветка на каждый этап (`feat/stage-1-services`), коммиты маленькими шагами.
- Оба сервера поднимаются одной командой через `concurrently`.
- Docs: Pro Git book гл. 1–3 (https://git-scm.com/book/ru/v2), Vite Getting Started (https://vite.dev/guide/).
- Done: репо создан, `npm run dev` поднимает заглушки клиента и сервера, README рассказывает, что за продукт.

### Этап 1. Сервер без базы: HTTP и REST на Express
- CRUD услуг в памяти (массив): GET/POST/PUT/DELETE `/api/services`.
- Статус-коды (200/201/204/400/404/409/500), JSON-ответы одного формата, обработка 404 и ошибок.
- Проверка руками: Thunder Client/Postman + коллекции запросов в репо.
- Docs: MDN HTTP overview (https://developer.mozilla.org/ru/docs/Web/HTTP/Guides/Overview),
  Express Routing + Basic routing (https://expressjs.com/en/guide/routing.html).
- Doc-задание: чем PUT отличается от PATCH? Зачем `express.json()`? Какой код вернуть при создании? При конфликте?
- Done: API работает, в репо лежит коллекция запросов и таблица эндпоинтов в README.

### Этап 2. Данные: PostgreSQL + Sequelize
- Модели Service, Master; миграции и сидеры через sequelize-cli; связи one-to-many.
- Подключение через `.env` (переменная `DB`), проверка `sequelize.authenticate()`.
- Заменить «массив в памяти» на реальные запросы к БД; пагинация LIMIT/OFFSET.
- Docs: Sequelize Getting Started (https://sequelize.org/docs/v6/getting-started/),
  Migrations (https://sequelize.org/docs/v6/tools/migrations/), PostgreSQL Tutorial (https://www.postgresqltutorial.com/).
- Doc-задание: что такое migration up/down и зачем down? Чем `timestamps: true` полезен? Что делает `paranoid`?
- Done: сервер переживает перезапуск, данные живут в Postgres, сиды накатываются одной командой.

### Этап 3. Клиент: React + TypeScript, FSD-lite
- Vite + React + TS; страницы: список услуг, форма услуги; роутинг.
- Слои как в `Sova`: `entities / features / pages / widgets / shared`, но bewusst упрощённо.
- fetch/axios-слой, состояния loading/error/empty, формы с валидацией.
- Docs: react.dev Learn «Thinking in React» (https://react.dev/learn/thinking-in-react),
  TS Handbook (https://www.typescriptlang.org/docs/handbook/intro.html).
- Сравнение с `Sova`: открыть `client/src/entities/task/*` и объяснить себе вслух, зачем каждый слой.
- Done: админка услуг работает в браузере, TS строгий (`strict: true`), ошибок компиляции нет.

### Этап 4. Авторизация и роли
- Регистрация/логин пользователя; роли: client / master / admin.
- JWT access + refresh, refresh в httpOnly-cookie; middleware проверки токена; гварды маршрутов на клиенте.
- Docs: jwt.io Introduction (https://jwt.io/introduction), MDN Cookies
  (https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Cookies),
  OWASP Authentication Cheat Sheet (https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html).
- Сравнение с `Sova`: `server/src/middleware/verifyAccessToken.js` и `generateTokens.js` — разобрать построчно и объяснить.
- Doc-задание: почему refresh хранят в httpOnly-cookie, а не в localStorage? Что такое CSRF и как мы от него защищены?
- Done: три роли видят разный интерфейс; постменом чужим токеном чужие данные не изменить.

### Этап 5. Ядро продукта: записи и расписание
- Модель Booking; проверка пересечения интервалов (никаких двойных записей); отмена записи.
- Расписание на день/неделю по мастеру; часовые пояса «по-взрослому» (хранить UTC).
- Docs: MDN Date (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date),
  Sequelize Transactions (https://sequelize.org/docs/v6/advanced-association-concepts/... и раздел Querying/Transactions).
- Done: два одновременных запроса на одно время → один успех, второй 409. Это твой главный рассказ на собеседовании.

### Этап 6. Продуктовый лоск
- Поиск/фильтры/пагинация на клиенте; тосты, скелетоны, пустые состояния.
- Личный кабинет клиента «мои записи», кабинет мастера «мой день».
- Простая аналитика админа: записи за неделю, выручка (заглушка цен услуг).
- Done: демо за 3 минуты «от гостя до записи» без единой ошибки в консоли.

### Этап 7. Качество: тесты и обработка ошибок
- Unit: утилиты расписания (Vitest). Integration: API-тесты авторизации и конфликта записи (Supertest).
- Централизованный error-handling middleware, логирование, единый формат ошибки.
- Docs: Vitest Guide (https://vitest.dev/guide/), Supertest (https://github.com/ladjs/supertest),
  Express Error handling (https://expressjs.com/en/guide/error-handling.html).
- Done: `npm test` зелёный, CI падает на красных тестах.

### Этап 8. Docker, CI, деплой
- docker-compose: server + client + postgres; Dockerfile'ы; переменные окружения.
- GitHub Actions: lint + test + build на каждый пуш.
- Деплой (Render/Railway/VPS — выберем вместе); живой URL в README.
- Docs: Docker Get started (https://docs.docker.com/get-started/), GitHub Actions docs (https://docs.github.com/en/actions).
- Done: проект открывается по ссылке с телефона.

### Этап 9. Упаковка: собеседование и питч бизнесу
- README-витрина: скриншоты, архитектура (схема), стек, «как запустить за 2 минуты», демо-данные.
- One-pager для бизнеса: «что получит салон» (напоминания клиентам, меньше неявок, учёт загрузки мастеров).
- Мок-собеседование: я гоняю тебя по собственному коду вопросами уровня джун+/мидл-.
- Done: репозиторий не стыдно отправить рекрутеру, а питч — владельцу салона.

---

## Быстрые улучшения старого проекта (`Sova`) — сделать за 15 минут, не откладывая

1. `git rm --cached client/.env` + добавить `.env` в `.gitignore` (в репо не должно быть env-файлов).
2. В корень — `package.json` со скриптом `dev` через `concurrently`, чтобы поднимать client+server одной командой.
3. В README: скриншоты, инструкция по `.env` (имя переменной `DB`!), список скриптов.
4. Сервер: валидировать env при старте (падать сразу с понятной ошибкой, если `DB`/секретов нет).

Эти правки — тоже упражнение: сделаешь сам, по документации `concurrently` и git book, я проверю.

---

## Журнал

Ведём в `learning/JOURNAL.md`. Формат записи: дата, этап, что сделал, что понял, что застряло, что читать дальше.
