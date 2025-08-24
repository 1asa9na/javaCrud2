# CRUD Project (Java + Maven + Liquibase + MySQL in Docker)

## 📦 Описание

Проект --- это пример CRUD-приложения на **Java (Maven)** с управлением
схемой базы данных через **Liquibase**.\
Инфраструктура разворачивается с помощью **Docker Compose**:\
- **MySQL** --- база данных\
- **Maven App** --- контейнер для сборки проекта и применения миграций
Liquibase

------------------------------------------------------------------------

## ⚙️ Требования

-   [Docker](https://www.docker.com/) ≥ 20.x\
-   [Docker Compose](https://docs.docker.com/compose/) ≥ v2\
-   (опционально) Maven локально, если нужно запускать сборку без Docker

------------------------------------------------------------------------

## 🚀 Запуск проекта

### 1. Клонирование репозитория

``` bash
git clone https://github.com/your-repo/crud-project.git
cd crud-project
```

### 2. Запуск контейнеров

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

## 📂 Полезные команды

### Остановить контейнеры

``` bash
docker compose down
```

### Удалить контейнеры и данные

``` bash
docker compose down -v
```

### Выполнить миграции вручную

``` bash
docker compose run --rm app mvn liquibase:update
```

### Проверить состояние базы

``` bash
docker compose exec db mysql -uappuser -papppass crud_db
```

------------------------------------------------------------------------

## 🛠️ Настройки подключения

База доступна:

-   Внутри Docker-сети: `jdbc:mysql://db:3306/crud_db`\
-   С хоста (например, для IDE): `jdbc:mysql://localhost:3307/crud_db`
    -   Пользователь: `appuser`\
    -   Пароль: `apppass`

------------------------------------------------------------------------

## 📌 Примечания

-   Все изменения схемы нужно добавлять в **Liquibase changelog**
    (`src/main/resources/db/changelog/db.changelog-master.xml`).\
-   Не используйте SQL-дампы (`initdb.sql`), так как схему теперь
    полностью контролирует Liquibase.\
-   Для продакшн-окружений можно вынести параметры подключения в
    отдельные `liquibase.properties` или передавать через
    `-Dliquibase.url=...`.
