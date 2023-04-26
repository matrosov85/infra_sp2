# API_YAMDB
REST API проект для сервиса YaMDb.

## Описание
Проект YaMDb собирает отзывы пользователей на произведения.


### Запуск проекта в контейнерах:

Клонировать репозиторий:
```
git clone <url>
```

Перейти в корневую директорию проекта:
```
cd api_yamdb
```

Создать и активать виртуальное окружение:
```
python -m venv venv
source /venv/Scripts/activate
```

Устаноить зависимости из файла requirements.txt:
```
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Перейти в папку с файлом docker-compose.yaml:
```
cd infra
```

Запустить контейнеры:
```
docker-compose up -d --build
```

Выполнить миграции:
```
docker-compose exec web python manage.py makemigrations
docker-compose exec web python manage.py migrate
```

Создать суперпользователя:
```
docker-compose exec web python manage.py createsuperuser
```

Собрать статику:
```
docker-compose exec web python manage.py collectstatic --no-input
```

Создать дамп базы данных:
```
docker-compose exec web python manage.py dumpdata > fixtures.json
```

Остановить контейнеры:
```
docker-compose down
```

### Шаблон наполнения .env файла
```
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
```

## Примеры запросов к API

### Получение списка всех произведений

[GET-запрос]:

```
.../api/v1/titles/
```

Доступно без токена.

Параметры запроса:
- **category** (string) фильтрует по полю slug категории
- **genre**	(string) фильтрует по полю slug жанра
- **name** (string) фильтрует по названию произведения
- **year** (integer) фильтрует по году

Ответ API - (**200**):

```
{
  "count": 0,
  "next": "string",
  "previous": "string",
  "results": [...]
}
```

### Регистрация нового пользователя

[POST-запрос]:

```
.../api/v1/auth/signup/
```

Присылает код подтверждения на переданный email. Код необходим для получения токена.

Body:

```
{
  "email" : "string", # Здесь ваши данные. До 254 символов, string
  "username" : "string" # Здесь ваши данные. До 150 символов, string
}
```

Ответ API - (**200**):

```
{
  "email": "string",
  "username": "string"
}
```

Другие возможные ответы:

- **400** Отсутствует обязательное поле или оно некорректно


### Добавление нового отзыва

Пользователь может оставить только один отзыв на произведение. Для доступа необходим jwt-токен.

[POST-запрос]:

```
.../api/v1/titles/{title_id}/reviews/
```

Параметры запроса:
- **title_id** (integer) ID произведения

Body:

```
{
  "text" : "example-text", # Текст отзыва, string
  "score" : [1...10] # Оценка в диапазоне от 1 до 10, integer
}
```

Ответ API - (**201**):

```
{
"id": 0,
"text": "example-text",
"author": "string",
"score": 1,
"pub_date": "2019-08-24T14:15:22Z"
}
```

Другие возможные ответы:

- **400** Отсутствует обязательное поле или оно некорректно
- **401** Необходим JWT-токен
- **404** Произведение не найдено
