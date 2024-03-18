# Kittygram
# Описание
# Технологии
- Python 3.9
- PostgreSQL
- Django
- Django REST Framework
- Nginx
- Docker
- Gunicorn
# Локальный запуск
Для запуска проекта необходимо установить [Docker](https://docs.docker.com/engine/install/)

Клонировать репозиторий:

```bash
git clone git@github.com:Chernovasilenko/kittygram_final
```
Создать в корневой директории .env файл со следующим содержанием:

```bash
# Django settings
SECRET_KEY=<secret_key>
DEBUG=False
ALLOWED_HOSTS=<host_ip_address>;127.0.0.1;localhost;<domain.name>

# DB
POSTGRES_USER=<user>
POSTGRES_PASSWORD=<password>
POSTGRES_DB=<db name>
DB_HOST=db
DB_PORT=5432

# Superuser data
ADMIN_USERNAME=<usename>
ADMIN_EMAIL=<admin@mail.com>
ADMIN_PASSWORD=<password>
```

# Запуск через Docker:

```bash
sudo docker compose -f docker-compose.dev.yml up
```
После  запуска проект доступен на локальном IP 127.0.0.1:9000






### Как запустить проект:

Клонировать репозиторий и перейти в него в командной строке:

```
git clone https://github.com/yandex-praktikum/kittygram_backend.git
```

```
cd kittygram_backend
```

Cоздать и активировать виртуальное окружение:

```
python3 -m venv env
```

* Если у вас Linux/macOS

    ```
    source env/bin/activate
    ```

* Если у вас windows

    ```
    source env/scripts/activate
    ```

```
python3 -m pip install --upgrade pip
```

Установить зависимости из файла requirements.txt:

```
pip install -r requirements.txt
```

Выполнить миграции:

```
python3 manage.py migrate
```

Запустить проект:

```
python3 manage.py runserver
```