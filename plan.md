## Пошаговая инструкция для проекта с БД

0. **Инициализация проекта**

```bash
npm init -y
```

Настройка файла `.gitignore` Создайте стандартный `.gitignore` для Node.js:

```bash
npx gitignore node
```

Настройка ESLint Установите и настройте линтер для кода JavaScript:

```bash
npm init @eslint/config
```

### ИЛИ установка пакета elbrus/config

```bash
npm init @elbrus/config@latest
```

1. **Установка Sequelize и необходимых пакетов**

   ```bash
   npm install sequelize pg pg-hstore
   ```

2. **Установка Sequelize CLI**

   ```bash
   npm i -D sequelize-cli
   ```

   ```
   Установить пакет dotenv
   npm install dotenv
   ```

3. **Создать файл `.sequelizerc` с настройками**

   ```js
   const path = require('path');
require('dotenv').config();

module.exports = {
  config: path.resolve('src', 'db', 'config', 'database.json'),
  'models-path': path.resolve('src', 'db', 'models'),
  'seeders-path': path.resolve('src', 'db', 'seeders'),
  'migrations-path': path.resolve('src', 'db', 'migrations'),
};
   ```

4. **Инициализация структуры проекта для базы данных**

   ```bash
   npx sequelize-cli init
   ```

   Если вдруг папки не создались DB то значит копируются лишние невидимые символы в файл и надо файл .sequelizerc создать через терминал и так же его заполнить через терминал и тогда лишних символов в нем не будут
   
   

5. **Заполнение файла `db/config/database.json` в папке db своими данными**  
   (например, подключение к вашей базе PostgreSQL)

6. **Создание базы данных**

   ```bash
   npx sequelize-cli db:create
   ```

7. **Создание модели и миграции для таблицы**

   ```bash
   npx sequelize model:generate --name Group --attributes title:string
   ```

8. **Накатить миграции**

   ```bash
   npx sequelize-cli db:migrate
   ```

9. **Откатить все миграции (если нет связей)**

   ```bash
   npx sequelize-cli db:migrate:undo:all
   ```

10. **Добавить связи в модели и миграциях**  
    (Редактируйте модель и миграционный файл, чтобы описать отношения между таблицами)

11. **Снова накатить миграции после исправлений**

    ```bash
    npx sequelize-cli db:migrate
    ```

12. **Создание файла генератора сидов**

    ```bash
    npx sequelize seed:generate --name init-seeds
    ```

13. **Заполнить в миграциях поля createdAt и updatedAt:**

    ```js
    defaultValue: Sequelize.fn('NOW');
    ```

14. **Заполнить таблицу сидами**

    ```bash
    npx sequelize db:seed:all
    ```

15. **Скрипты для управления БД (добавить в package.json):**

    ```json
    "db": "npx sequelize-cli db:drop && npx sequelize-cli db:create && npx sequelize-cli db:migrate && npx sequelize-cli db:seed:all",
    "db:remigrate": "npx sequelize db:migrate:undo:all && npx sequelize db:migrate && npx sequelize db:seed:all"
    ```

16. **Для визуализации структуры БД**  
    Используйте сервис [drawsql.app](https://drawsql.app/)
