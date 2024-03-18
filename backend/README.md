Клонировать репозиторий и перейти в него в командной строке: 
 
```bash
git clone git@github.com:Chernovasilenko/kittygram_final
```
 
 Создать в корневой директории .env файл со следующим содержанием:

```
# Django settings
SECRET_KEY=<secret_key>
DEBUG=<True/False>
ALLOWED_HOSTS=<host_ip_address>;127.0.0.1;localhost;<domain.name>
# Оставляем True - если будем работать в SQLite,
# False - если в POSTGRES (для этого нужно будет установить POSTGRES).
USE_SQLITE=True

# POSTGRES data
POSTGRES_USER=<user>
POSTGRES_PASSWORD=<password>
POSTGRES_DB=<db name>
DB_HOST=db
DB_PORT=5432
```

```bash
cd kittygram_backend 
``` 
 
Cоздать и активировать виртуальное окружение: 
 
```bash
python3 -m venv env 
``` 
 
* Если у вас Linux/macOS 
 
    ```bash
    source env/bin/activate 
    ``` 
 
* Если у вас windows 
 
    ```bash
    source env/scripts/activate 
    ``` 
 
```bash
python3 -m pip install --upgrade pip 
``` 
 
Установить зависимости из файла requirements.txt: 
 
```bash
pip install -r requirements.txt 
``` 
 
Выполнить миграции: 
 
```bash
python3 manage.py migrate 
``` 
 
Запустить проект: 
 
```bash
python3 manage.py runserver 
``` 