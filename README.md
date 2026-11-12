# Learning Log

Веб-приложение **Learning Log** для ведения журнала изучаемых тем и записей по каждой теме.

Проект разработан на **Django** и предназначен для запуска как локально, так и на сервере с использованием **Docker Compose**.

## Используемые технологии

* Python
* Django 6.1.1
* SQLite
* django-bootstrap4
* WhiteNoise
* Docker
* Docker Compose

## Структура проекта

Основные файлы и каталоги проекта:

```text
learning_log/
├── learning_log/          # настройки и конфигурация Django
├── learning_logs/         # основное приложение Learning Log
├── users/                 # приложение пользователей
├── manage.py              # файл управления Django
├── requirements.txt       # зависимости Python
├── Dockerfile             # инструкция создания Docker-образа
├── compose.yaml           # конфигурация Docker Compose
├── README.md              # инструкция по проекту
└── .gitignore             # файлы, исключённые из Git
```

## Требования

Для запуска проекта на сервере необходимо установить:

* Docker
* Docker Compose

Современные версии Docker Desktop уже содержат Docker Compose.

Проверить установку Docker можно командами:

```bash
docker --version
docker compose version
```

## Клонирование репозитория

Склонируйте репозиторий:

```bash
git clone <ССЫЛКА_НА_ВАШ_РЕПОЗИТОРИЙ>
```

Перейдите в каталог проекта:

```bash
cd learning_log
```

## Переменные окружения

Файл `.env` не должен загружаться в GitHub.

Перед запуском проекта необходимо создать файл `.env` в корневой папке проекта.

Пример:

```env
DJANGO_SECRET_KEY=your-secret-key
DJANGO_DEBUG=False
DJANGO_ALLOWED_HOSTS=localhost,127.0.0.1
DJANGO_CSRF_TRUSTED_ORIGINS=
SQLITE_PATH=/app/data/db.sqlite3
```

### Описание переменных

| Переменная                    | Назначение                     | Пример                 |
| ----------------------------- | ------------------------------ | ---------------------- |
| `DJANGO_SECRET_KEY`           | Секретный ключ Django          | `your-secret-key`      |
| `DJANGO_DEBUG`                | Режим отладки                  | `False`                |
| `DJANGO_ALLOWED_HOSTS`        | Разрешённые домены и IP-адреса | `localhost,127.0.0.1`  |
| `DJANGO_CSRF_TRUSTED_ORIGINS` | Доверенные источники CSRF      | `https://example.com`  |
| `SQLITE_PATH`                 | Путь к базе SQLite             | `/app/data/db.sqlite3` |

Для реального сервера в `DJANGO_ALLOWED_HOSTS` необходимо указать доменное имя или IP-адрес сервера.

Например:

```env
DJANGO_ALLOWED_HOSTS=example.com,www.example.com
```

Если используется HTTPS:

```env
DJANGO_CSRF_TRUSTED_ORIGINS=https://example.com,https://www.example.com
```

## Запуск проекта через Docker

После установки Docker и создания `.env` выполните в корневой папке проекта:

```bash
docker compose up -d --build
```

Команда автоматически:

1. создаст Docker-образ проекта;
2. установит необходимые зависимости;
3. создаст контейнер;
4. запустит Django;
5. запустит приложение в фоновом режиме.

Проверить запущенные контейнеры:

```bash
docker compose ps
```

Посмотреть логи:

```bash
docker compose logs -f
```

После успешного запуска приложение будет доступно по адресу:

```text
http://localhost:8000/
```

## Выполнение миграций

Если миграции не выполняются автоматически при запуске контейнера, выполните:

```bash
docker compose exec web python manage.py migrate
```

После этого снова откройте:

```text
http://localhost:8000/
```

## Создание администратора

Для создания пользователя администратора выполните:

```bash
docker compose exec web python manage.py createsuperuser
```

Следуйте инструкциям в терминале.

После создания администратора панель управления Django доступна по адресу:

```text
http://localhost:8000/admin/
```

## Сбор статических файлов

Для сборки статических файлов выполните:

```bash
docker compose exec web python manage.py collectstatic --noinput
```

Статические файлы будут собраны в каталог:

```text
staticfiles/
```

## Остановка проекта

Чтобы остановить контейнеры:

```bash
docker compose down
```

Для повторного запуска:

```bash
docker compose up -d
```

Если были изменены исходные файлы, зависимости или Docker-конфигурация:

```bash
docker compose up -d --build
```

## Просмотр логов

Для просмотра логов проекта:

```bash
docker compose logs -f web
```

Для выхода из просмотра логов нажмите:

```text
Ctrl + C
```

## Обновление проекта

После получения новых изменений из GitHub:

```bash
git pull
```

Затем пересоберите и перезапустите контейнер:

```bash
docker compose up -d --build
```

При необходимости выполните миграции:

```bash
docker compose exec web python manage.py migrate
```

## Безопасность

Следующие файлы и данные **не должны загружаться в GitHub**:

* `.env`
* секретные ключи;
* пароли;
* реальные пользовательские данные;
* база данных с реальными данными;
* виртуальное окружение Python;
* временные файлы;
* кэш Python;
* файлы, создаваемые Docker во время работы.

В `.gitignore` рекомендуется добавить:

```gitignore
# Python
__pycache__/
*.py[cod]
*.pyo

# Virtual environment
ll_env/
venv/
.venv/

# Environment variables
.env
.env.*

# SQLite database
*.sqlite3
db.sqlite3

# Django
staticfiles/

# IDE
.idea/
.vscode/

# OS
.DS_Store
Thumbs.db
```

## Локальный запуск без Docker

Для запуска проекта непосредственно в виртуальном окружении Python:

### Windows

Активировать виртуальное окружение:

```powershell
.\ll_env\Scripts\Activate.ps1
```

Установить зависимости:

```powershell
pip install -r requirements.txt
```

Выполнить миграции:

```powershell
python manage.py migrate
```

Запустить сервер:

```powershell
python manage.py runserver
```

После этого приложение доступно по адресу:

```text
http://127.0.0.1:8000/
```

## Основные возможности

Приложение позволяет:

* регистрировать пользователей;
* выполнять вход и выход из системы;
* создавать темы обучения;
* просматривать список собственных тем;
* добавлять записи к темам;
* редактировать записи;
* хранить данные в базе данных;
* работать с приложением через веб-браузер.

## Лицензия

Учебный проект. Используется в образовательных целях.
