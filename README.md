# Kittygram
# Описание
Kittygram - социальноая сеть для обмена фотографиями любимых питомцев. 
Пользователи могут загружать фотографии своих котиков, указывать дату рождения, цвет и достижения своих питомцев. 
Зарегистрированные пользователи могут просматривать фотографии котов других пользователей.
Для запуска проекта необходимо установить [Docker](https://docs.docker.com/engine/install/).
# Технологии
- Python 3.9
- Django
- Django REST Framework
- PostgreSQL
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
USE_SQLITE=False

# POSTGRES data
POSTGRES_USER=<user>
POSTGRES_PASSWORD=<password>
POSTGRES_DB=<db name>
DB_HOST=db
DB_PORT=5432
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
USE_SQLITE=False

# POSTGRES data
POSTGRES_USER=<user>
POSTGRES_PASSWORD=<password>
POSTGRES_DB=<db name>
DB_HOST=db
DB_PORT=543

# Docker 
DOCKER_USERNAME=<docker_username>
```
Перенести на сервер файлы .env и docker-compose.production.yml

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

Настроить конфиг nginx на сервере, чтобы все зпросы на порт `9000` перенаправлялись в докер:

```bash
sudo nano /etc/nginx/sites-enabled/default
```

```
location / {
    proxy_set_header Host $http_host;
    proxy_pass http://127.0.0.1:9000;
}
```

После изменений перезапустить nginx:
```bash
sudo systemctl restart nginx.service
```

# CI/CD

Для использования CI/CD через GitHub Actions необходимо добавить следующие значения в Actions secrets:
- `DOCKER_USERNAME` | Никнейм в Docker
- `DOCKER_PASSWORD` | Пароль в Docker
- `HOST` | ip-адрес сервера
- `USER` | Имя юзера на сервере
- `SSH_KEY` | SSH_KEY
- `SSH_PASSPHRASE` | SSH пароль
- `TELEGRAM_TO` | id телеграм-аккаунта
- `TELEGRAM_TOKEN` | токен телеграм-бота

При использовании `git push` в ветку `main` будет происходить автоматическое тестирование кода, сборка и загрузка образов, деплой на сервер и отправка сообщения в телеграм при успешном деплое.