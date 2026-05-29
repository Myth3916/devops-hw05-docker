## Домашнее задание к занятию 5. «Практическое применение Docker» - Шаров Олег

```markdown
# 🐳 DevOps Homework 05: Docker, Compose & Cloud Deployment

##  Описание
Учебный проект, демонстрирующий навыки контейнеризации, оркестрации, облачного деплоя и оптимизации Docker-образов.
Веб-приложение на **FastAPI + MySQL** развёрнуто в изолированной Docker-сети, доступно через цепочку прокси (Nginx → HAProxy), автоматически деплоится на ВМ в Yandex Cloud, а база данных регулярно бэкапится.
```
## ️ Архитектура

 Пользователь
    ↓ (port 8090)
🟢 Nginx (Ingress Proxy)
    ↓ (port 8080)
🟡 HAProxy (Reverse Proxy)
    ↓ (port 5000)
🐍 FastAPI App (Python 3.12)
    ↓
 MySQL 8 (Database)
```
Все сервисы находятся в сети `backend` с фиксированными IP-адресами (`web: 172.20.0.5`, `db: 172.20.0.10`).

##  Структура проекта
```text
├── .dockerignore          # Исключения для Docker
├── .gitignore             # Исключения для Git
── .env                   # Переменные окружения (БД)
├── Dockerfile.python      # Multistage сборка Python-приложения
├── Dockerfile.terraform   # Оптимизированный образ Terraform
├── compose.yaml           # Оркестрация всех сервисов
├── proxy.yaml             # Конфигурация Nginx и HAProxy
├── haproxy/reverse/       # Конфиг HAProxy
├── nginx/ingress/         # Конфиг Nginx
├── main.py                # Исходный код FastAPI
├── requirements.txt       # Зависимости Python
├── schema.pdf             # Схема БД
└── README.md              # Документация
```

## 🚀 Локальный запуск
1. Убедитесь, что Docker и Docker Compose установлены.
2. Настройте переменные окружения в `.env` (пароли MySQL).
3. Запустите проект:
   ```bash
   docker compose up -d --build
   ```
4. Проверьте работу:
   ```bash
   curl -L http://127.0.0.1:8090
   # Ожидаемый ответ: "TIME: ..., IP: ..."
   ```

## ☁️ Деплой в Yandex Cloud (Задача 4)
Проект развёрнут на ВМ (Ubuntu 22.04, 2 vCPU, 2GB RAM, 20% доля).
- **Скрипт деплоя:** `/opt/deploy.sh`
- Автоматически клонирует репозиторий по SSH и запускает `docker compose up -d --build`.
- Доступ проверен через [check-host.net](https://check-host.net) (код `200 OK` со всех узлов мира).

## 💾 Резервное копирование БД (Задача 5)
- **Скрипт бэкапа:** `/opt/backup.sh`
- Использует `docker exec` для создания дампа MySQL без утечки паролей в Git.
- **Расписание:** Cron запускает скрипт каждую минуту (`* * * * *`).
- **Хранилище:** `/opt/backup/backup_YYYY-MM-DD_HH-MM-SS.sql`
- Секреты хранятся в `/home/yc-user/.db_secrets` (добавлены в `.gitignore`).

## 📦 Оптимизация образа Terraform (Задача 6)
Проведён анализ официального образа `hashicorp/terraform:latest` с помощью утилиты `dive`.
- **Исходный размер:** 146 MB
- **Оптимизированный размер:** 84.7 MB (**экономия 42%**)
- **Image Efficiency Score:** 100%
- **Potential wasted space:** 0 B
- Использован `FROM scratch` + multistage сборка. В финальном образе остался только бинарник `terraform` и необходимые SSL-сертификаты.

## 📸 Скриншоты для отчёта
| Задача | Описание | Имя файла |
|--------|----------|-----------|
| 1 | Структура, Dockerfile, .dockerignore, .gitignore | `task1_*.png` |
| 1 | Успешная сборка и запуск контейнера | `task1_build_success.png`, `task1_app_run.png` |
| 3 | Запуск compose, curl-тест, SQL-запрос к БД | `task3_*.png` |
| 4 | ВМ в консоли YC, SSH, скрипт деплоя | `task4_vm_*.png`, `task4_deploy_*.png` |
| 4 | Проверка доступности (check-host.net) | `task4_check_host.png` |
| 5 | Скрипт бэкапа, cron, файлы дампов | `task5_*.png` |
| 6 | Анализ dive, сравнение размеров образов | `task6_*.png` |

## ️ Технологии
- **Backend:** Python 3.12, FastAPI, Uvicorn
- **Database:** MySQL 8
- **Proxy:** Nginx (Ingress), HAProxy (Reverse)
- **Infra:** Docker, Docker Compose, Yandex Cloud Compute, Bash, Cron
- **Tools:** Dive, Git, SSH

## 👤 Автор
Олег Шаров | `os127@yandex.ru` | GitHub: [@Myth3916](https://github.com/Myth3916)
```
