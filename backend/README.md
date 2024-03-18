# Kittygram
# Описание
Для запуска проекта необходимо установить [Docker](https://docs.docker.com/engine/install/)
# Технологии
- Python 3.9
- PostgreSQL
- Django
- Django REST Framework
- Nginx
- Docker
- Gunicorn
# Локальный запуск
Клонировать репозиторий:

```bash
git clone git@github.com:Chernovasilenko/kittygram_final
```
Создать в корневой директории .env файл со следующим содержанием:

```
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

Запуск:

```bash
sudo docker compose -f docker-compose.dev.yml up
```
После  запуска проект доступен на локальном IP 127.0.0.1:9000

# Запуск на сервере
Клонировать репозиторий:

```bash
git clone git@github.com:Chernovasilenko/kittygram_final
```

Перейти в каталог проекта:

```bash
cd kittygram
```

Создать docker images образы:

```bash
sudo docker build -t <username>/kittygram_backend backend/
sudo docker build -t <username>/kittygram_frontend frontend/
sudo docker build -t <username>/kittygram_gateway nginx/
```

Загрузить образы на Docker Hub:

```bash
sudo docker push <username>/kittygram_backend
sudo docker push <username>/kittygram_frontend
sudo docker push <username>/kittygram_gateway
```

Создать .env файл со следующим содержанием:

```
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
Перенести на удаленный сервер файлы .env и docker-compose.production.yml

На сервере перейти в директорию kittygram:
```bash
cd kittygram
```

Выполнить сборку приложений:
```bash
sudo docker compose -f docker-compose.production.yml up -d
```

Выполнить миграции:
```bash
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
```

Собрать статику:
```bash
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
```

Скопировать статику:

```bash
sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. web/backend_static/static
```

Настроить шлюз для перенаправления запросов на `9000` порт, который слушает контейнер `kittygram_gateway`:

```bash
sudo nano /etc/nginx/sites-enabled/default
```

После изменений перезапустить nginx:
```bash
sudo systemctl restart nginx.service
```










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

# CI/CD

Для использования CI/CD через GitHub Actions необходимо добавить следующие значения в Actions secrets:
- `DOCKER_USERNAME`
- `DOCKER_PASSWORD`
- `HOST`
- `USER`
- `SSH_KEY`
- `SSH_PASSPHRASE`
- `TELEGRAM_TO`
- `TELEGRAM_TOKEN`