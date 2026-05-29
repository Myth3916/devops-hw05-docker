## Домашнее задание к занятию 5. «Практическое применение Docker» - Шаров Олег

## 👤 Студент
Олег Шаров | `os127@yandex.ru` | GitHub: [@Myth3916](https://github.com/Myth3916)

```markdown
# 🐳 DevOps Homework 05: Docker, Compose & Cloud Deployment

## 📋 Описание
Учебный проект, демонстрирующий навыки контейнеризации, оркестрации, облачного деплоя и оптимизации Docker-образов. Веб-приложение на **FastAPI + MySQL** развёрнуто в изолированной Docker-сети, доступно через цепочку прокси (Nginx → HAProxy), автоматически деплоится на ВМ в Yandex Cloud, а база данных регулярно бэкапится.
```

## 🏗️ Архитектура
```
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

---

## ✅ Задача 0: Проверка Docker Compose

![Docker Compose version](img/task0_docker_compose_check.png)

---

## ✅ Задача 1: Dockerfile и сборка приложения

### Структура проекта
![Project structure](img/task1_project_structure.png)

### Dockerfile.python (multistage сборка)
![Dockerfile](img/task1_dockerfile_python.png)

### .dockerignore
![Dockerignore](img/task1_dockerignore.png)

### .gitignore
![Gitignore](img/task1_gitignore.png)

### Успешная сборка
![Build success](img/task1_build_success.png)

### Запуск приложения
![App run](img/task1_app_run.png)

### Git commit
![Git commit](img/task1_git_commit.png)

---

## ✅ Задача 3: Docker Compose оркестрация

### SQL-запрос к базе данных
![SQL query](img/task3_sql_query.png)

---

## ✅ Задача 4: Деплой в Yandex Cloud

### Создание ВМ
![VM created](img/task4_vm_created.png)

### Подключение по SSH
![VM SSH](img/task4_vm_ssh.png)

### Установка Docker
![Docker install](img/task4_docker_install.png)

### Скрипт деплоя
![Deploy script](img/task4_deploy_script.png)

### Запуск деплоя
![Deploy run](img/task4_deploy_run.png)

### Статус контейнеров
![Containers status](img/task4_containers_status.png)

### Проверка локально (на ВМ)
![Curl local](img/task4_curl_local.png)

### Проверка удалённо (с локальной машины)
![Curl remote](img/task4_curl_remote.png)

### Проверка доступности (check-host.net)
![Check host](img/task4_check_host.png)

---

## ✅ Задача 5: Резервное копирование БД

### Скрипт бэкапа
![Backup script](img/task5_backup_script.png)

### Настройка Cron
![Cron config](img/task5_cron_config.png)

### Файлы бэкапов
![Backups](img/task5_backups.png)

### Содержимое бэкапа
![Backup content](img/task5_backup_content.png)

---

## ✅ Задача 6: Оптимизация образа Terraform

### Сравнение размеров образов
![Image comparison](img/task6_image_comparison.png)

### Анализ оптимизированного образа (100% efficiency!)
![Dive optimized](img/task6_dive_optimized.png)

### Проверка работы Terraform
![Terraform works](img/task6_terraform_works.png)

---

## 🚀 Локальный запуск
1. Убедитесь, что Docker и Docker Compose установлены
2. Настройте переменные окружения в `.env`
3. Запустите проект:
   ```bash
   docker compose up -d --build
   ```
4. Проверьте работу:
   ```bash
   curl -L http://127.0.0.1:8090
   ```

## 📦 Технологии
- **Backend:** Python 3.12, FastAPI, Uvicorn
- **Database:** MySQL 8
- **Proxy:** Nginx, HAProxy
- **Infra:** Docker, Docker Compose, Yandex Cloud
- **Tools:** Dive, Git, Bash, Cron

```