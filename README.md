# CRUD Project (Java + Maven + Liquibase + MySQL in Docker)

## Описание

- **MySQL** --- база данных\
- **Maven App** --- контейнер для сборки проекта и применения миграций
Liquibase

------------------------------------------------------------------------

## Запуск проекта

### Запуск контейнеров

``` bash
docker compose up --build
```

При старте:\
- Контейнер **MySQL** создаст пустую базу `crud_db`.\
- Контейнер **Maven** выполнит: - `mvn clean package` -
`mvn liquibase:update` → применит все миграции из
`db/changelog/db.changelog-master.xml`.

------------------------------------------------------------------------

## 🗄️ Структура базы данных

Схема создаётся **Liquibase** автоматически:

-   **writers** (id, first_name, last_name)\
-   **posts** (id, content, created, updated, status, writer_id) → FK →
    writers(id)\
-   **labels** (id, name) \[UNIQUE\]\
-   **post_labels** (post_id, label_id) → FK → posts(id), labels(id)

------------------------------------------------------------------------

## 🛠️ Настройки подключения

База доступна:

-   Внутри Docker-сети: `jdbc:mysql://db:3306/crud_db`\
-   С хоста (например, для IDE): `jdbc:mysql://localhost:3307/crud_db`
    -   Пользователь: `appuser`\
    -   Пароль: `apppass`
