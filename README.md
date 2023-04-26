# API_YAMDB
REST API проект для сервиса YaMDb.

## Описание
Проект YaMDb собирает отзывы пользователей на произведения.


### Запуск проекта в контейнерах:

Клонировать репозиторий:
```bash
git clone <url>
```

Перейти в корневую директорию проекта:
```bash
cd api_yamdb
```

Создать и активать виртуальное окружение:
```bash
python -m venv venv
source /venv/Scripts/activate
```

Устаноить зависимости из файла requirements.txt:
```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Перейти в папку с файлом docker-compose.yaml:
```bash
cd infra
```

Запустить контейнеры:
```bash
docker-compose up -d --build
```

Выполнить миграции:
```bash
docker-compose exec web python manage.py makemigrations
docker-compose exec web python manage.py migrate
```

Создать суперпользователя:
```bash
docker-compose exec web python manage.py createsuperuser
```

Собрать статику:
```bash
docker-compose exec web python manage.py collectstatic --no-input
```

Создать дамп базы данных:
```bash
docker-compose exec web python manage.py dumpdata > fixtures.json
```

Остановить контейнеры:
```bash
docker-compose down -v
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
