### **Часть 1: Что такое Docker и зачем он нужен**

**Docker — это платформа для разработки, доставки и запуска приложений в изолированных, легковесных средах, называемых контейнерами.** Если говорить просто, это инструмент, который упаковывает ваше приложение со всеми его зависимостями (библиотеки, система выполнения, конфигурации) в стандартную, портативную единицу — **контейнер**. Этот контейнер гарантированно будет работать одинаково на любой системе, где есть Docker: на вашем ноутбуке с macOS, на сервере с Ubuntu в облаке или в продакшен-кластере.

#### Ключевые концепции, которые формируют его суть:

1.  **Образ (Image)**
    *   **Шаблон, инструкция, слепок.** Это неизменяемый файл, на основе которого создаются контейнеры. Образ включает в себя всё необходимое для запуска приложения: код, среду выполнения, системные инструменты, библиотеки.
    *   Образы строятся послойно. Каждая инструкция в файле `Dockerfile` создает новый слой. Это делает сборку эффективной и позволяет переиспользовать слои (например, базовый образ Ubuntu используется для тысяч разных приложений).
    *   Образы хранятся и распространяются через **реестры**, самый известный из которых — **Docker Hub** (аналог GitHub для образов).

2.  **Контейнер (Container)**
    *   **Запущенный экземпляр образа.** Это изолированный, работающий процесс, созданный из образа. Контейнеры легковесны, потому что они используют ядро основной операционной системы хоста (Linux), а не запускают полноценную гостевую ОС, как виртуальные машины. Они запускаются за секунды.
    *   Контейнеры обеспечивают изоляцию на уровне процессов, файловой системы и сети, но при этом могут взаимодействовать друг с другом и внешним миром через настроенные каналы.

3.  **Dockerfile**
    *   **Сердце и рецепт.** Это простой текстовый файл, содержащий пошаговые инструкции по сборке образа. В нём вы описываете, какой базовый образ взять, какие файлы скопировать, какие команды выполнить для установки зависимостей и как запустить приложение.
    *   Пример:
        ```dockerfile
        FROM python:3.9-slim
        WORKDIR /app
        COPY requirements.txt .
        RUN pip install -r requirements.txt
        COPY . .
        CMD ["python", "app.py"]
        ```

4.  **Docker Compose**
    *   **Инструмент для оркестровки нескольких контейнеров.** В реальных проектах редко используется один контейнер. Обычно есть приложение, база данных, кэш, веб-сервер. Docker Compose позволяет описать всю эту многоконтейнерную среду в одном файле `docker-compose.yml` и управлять ей одной командой (`docker-compose up`).

#### **Практическая ценность для IT-специалиста:**

*   **Устранение проблемы «Работает на моей машине»:** Среда в контейнере идентична у всех: у разработчика, тестировщика и на продакшене.
*   **Мгновенный онбординг:** Новый член команды запускает весь стек проекта одной командой, минуя дни настройки окружения.
*   **Чистота и изоляция:** На основной системе не нужно устанавливать множество версий языков и сервисов. Всё живёт в контейнерах.
*   **Основа для DevOps и CI/CD:** Образ, собранный на этапе тестирования, в неизменном виде разворачивается в продакшене. Это основа практик непрерывной интеграции и доставки.
*   **Эффективное использование ресурсов:** На одном сервере можно запустить десятки контейнеров вместо нескольких громоздких виртуальных машин.

По сути, Docker — это стандартизация процесса **упаковки** и **запуска** приложений. Его появление решило одну из самых старых и болезненных проблем разработки и системного администрирования.

---

### **Часть 2: История Docker**

История Docker — это классическая история о том, как **нужная абстракция, представленная в правильный момент, переворачивает индустрию**. Это путь от внутреннего скрипта стартапа до фундамента облачных вычислений.

#### **Предпосылки (2000-е – начало 2010-х)**

*   **Контекст:** Мир страдал от неэффективности виртуальных машин (тяжелые, медленные) и проблем переносимости приложений.
*   **Технологический фундадент:** В ядре Linux уже существовали ключевые технологии, которые позже станут основой контейнеров: **cgroups** (для контроля ресурсов, разработан в Google) и **namespaces** (для изоляции).
*   **Предшественники:** Существовали более низкоуровневые инструменты контейнеризации, такие как **LXC (Linux Containers)**, но они были сложны в использовании и не предлагали удобной модели упаковки приложений.

#### **Рождение и взрывная популярность (2013-2014)**

*   **Место рождения:** Французский стартап **dotCloud** (PaaS-платформа). Основатель **Соломон Хайкс (Solomon Hykes)** и его команда создали внутренний инструмент для решения собственных проблем с развертыванием тысяч разных приложений.
*   **Публичная презентация:** В **марте 2013 года** на конференции PyCon Хайкс представил этот инструмент миру под названием **Docker**. Его гениальность была не в низкоуровневой изоляции (она уже была в LXC), а в трех верхнеуровневых инновациях:
    1.  **Формат образа и Dockerfile:** Простой способ описывать и собирать приложение в переносимые слои.
    2.  **Дружелюбный CLI:** Команды `docker run`, `docker build` были понятны любому разработчику.
    3.  **Публичный реестр (Docker Hub):** Централизованное хранилище готовых образов (`nginx`, `redis`, `ubuntu`), которое запустило сетевой эффект.
*   **Резонанс:** Сообщество разработчиков, измученное «dependency hell», приняло Docker с невероятным энтузиазмом. За год проект набрал десятки тысяч звёзд на GitHub. Компания сменила название с dotCloud на **Docker, Inc.**

#### **Взросление, стандартизация и «Война оркестраторов» (2014-2017)**

*   **Docker 1.0 (июнь 2014):** Заявление о готовности к промышленному использованию.
*   **Отказ от LXC:** Docker заменил LXC собственной библиотекой `libcontainer`, чтобы получить полный контроль над стеком.
*   **Расширение экосистемы:** Появились **Docker Compose** (для многоконтейнерных приложений) и **Docker Swarm** (собственный инструмент оркестрации кластеров).
*   **Ключевой поворот – Open Container Initiative (OCI, 2015):** Чтобы избежать фрагментации экосистемы (как в «войнах дистрибутивов» Linux), Docker, Inc. вместе с гигантами вроде Google, Microsoft, IBM создали открытый консорциум OCI. Docker пожертвовал свои ключевые компоненты (`runc`, спецификации формата образа) для создания открытых стандартов. Это был стратегический ход, сделавший Docker-образы индустриальным стандартом.
*   **Война за оркестрацию:** Когда контейнеров стало много, возник вопрос управления. На арену вышли три соперника: **Docker Swarm** (простой, от Docker), **Apache Mesos** (зрелый) и **Kubernetes** (от Google, на основе внутренней системы Borg). К 2017-2018 годам сообщество и крупные облачные провайдеры сделали выбор в пользу **Kubernetes** как более мощного и гибкого решения.

#### **Современная эпоха: Фундамент экосистемы (2018 – настоящее время)**

*   **Победа Kubernetes:** Docker проиграл битву за оркестрацию, но выиграл войну за стандарт. **Формат OCI-образов, созданный Docker, стал основой, на которой работает вся экосистема Kubernetes и облачных вычислений.**
*   **Изменение фокуса Docker, Inc.:** Компания сосредоточилась на коммерческих продуктах для корпоративных разработчиков и, что важнее, на **Docker Desktop** — удобном графическом приложении для macOS и Windows, которое стало незаменимым инструментом локальной разработки.
*   **Текущая роль:** Сегодня Docker — это **инфраструктурный пласт**, как `git`. Вы можете не использовать Docker Engine в продакшене (там, скорее всего, будет `containerd` или `cri-o` внутри Kubernetes), но вы 100% будете использовать образы в формате OCI и, вероятно, Docker Desktop для сборки и тестирования. Он превратился из революционной новинки в невидимый, но абсолютно необходимый фундамент мира **cloud-native**.

### **Часть 3: Docker compose**

## **Что это и зачем нужно?**

**Docker Compose** — это инструмент для определения и запуска **многоконтейнерных Docker-приложений**. Если вы работаете с одним контейнером — вам хватит `docker run`. Если у вас несколько взаимосвязанных сервисов (приложение + БД + кэш + очередь) — вам нужен Compose.

### Простая аналогия
- **`docker run`** — как запустить один инструмент (гитару)
- **`docker-compose`** — как запустить целый оркестр (гитара, барабаны, клавиши, вокал)

## **Основной файл: `docker-compose.yml`**

Это YAML-файл, в котором вы описываете всю инфраструктуру вашего приложения.

### Базовый пример — веб-приложение с БД
```yaml
version: '3.8'  # Версия схемы Compose

services:  # Секция с сервисами (контейнерами)
  
  # Сервис 1: Веб-приложение
  webapp:
    build: .  # Собрать из Dockerfile в текущей директории
    ports:
      - "8000:8000"  # Хост:Контейнер
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/mydb
    depends_on:
      - database  # Запустить после database
    volumes:
      - ./app:/code  # Монтирование кода для разработки
    networks:
      - app-network

  # Сервис 2: База данных PostgreSQL
  database:
    image: postgres:15  # Использовать готовый образ
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: mydb
    volumes:
      - postgres_data:/var/lib/postgresql/data  # Постоянное хранилище
    networks:
      - app-network

  # Сервис 3: Redis для кэша
  cache:
    image: redis:7-alpine
    command: redis-server --appendonly yes
    networks:
      - app-network

  # Сервис 4: Nginx как reverse proxy
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - webapp
    networks:
      - app-network

# Определение сетей
networks:
  app-network:
    driver: bridge  # Тип сети

# Определение томов для постоянного хранения
volumes:
  postgres_data:  # Именованный том
```

---

## **Ключевые секции файла Compose**

### 1. **`services`** — сердце Compose
Каждый сервис = один контейнер. Основные параметры:

| Параметр | Пример | Описание |
|----------|--------|----------|
| `build` | `build: .` или `build: ./dir` | Собрать из Dockerfile |
| `image` | `image: nginx:alpine` | Использовать готовый образ |
| `ports` | `- "80:80"` или `- "3000-3005:3000-3005"` | Проброс портов |
| `environment` | `DATABASE_URL: ...` или файл `.env` | Переменные окружения |
| `volumes` | `- ./data:/app/data` | Монтирование томов |
| `depends_on` | `- db` | Зависимости запуска |
| `networks` | `- frontend` | Подключение к сетям |
| `command` | `command: python app.py` | Переопределить CMD |
| `healthcheck` | (см. ниже) | Проверка здоровья |

### 2. **`networks`** — изоляция и соединение
```yaml
networks:
  frontend:
    driver: bridge
    ipam:
      config:
        - subnet: 172.20.0.0/16
  backend:
    driver: bridge
```

**Сервисы в одной сети:**
- Могут общаться по **имени сервиса** (`ping database`)
- Видят друг друга на внутренних портах (`database:5432`)
- Изолированы от других сетей

### 3. **`volumes`** — управление данными
```yaml
volumes:
  # Анонимный том (управляется Docker)
  - /var/lib/mysql
  
  # Именованный том
  - db_data:/var/lib/postgresql/data
  
  # Bind mount (хостовая директория)
  - ./logs:/app/logs
  
  # Volume с настройками
  cachedata:
    driver: local
    driver_opts:
      type: tmpfs
      device: tmpfs
```

---

## **Продвинутые возможности**

### 1. **Переменные окружения и `.env` файл**
**docker-compose.yml:**
```yaml
services:
  app:
    image: myapp:${TAG:-latest}  # Значение по умолчанию
    environment:
      - DB_HOST=${DB_HOST}
      - DB_PORT=${DB_PORT:-5432}  # Значение по умолчанию
```

**.env файл (рядом с docker-compose.yml):**
```env
TAG=v2.0
DB_HOST=database
DB_PORT=5432
SECRET_KEY=mysecret
```

### 2. **Расширенные конфигурации**
```yaml
services:
  app:
    # Ресурсы контейнера
    deploy:
      resources:
        limits:
          cpus: '0.50'
          memory: 512M
        reservations:
          cpus: '0.25'
          memory: 256M
    
    # Перезапуск при падении
    restart: unless-stopped
    
    # Логирование
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"
    
    # Health check
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s
```

### 3. **Шаблонизация с `extends`**
**base.yml:**
```yaml
services:
  base:
    image: ubuntu:22.04
    environment:
      - COMMON_VAR=value
```

**docker-compose.yml:**
```yaml
services:
  web:
    extends:
      file: base.yml
      service: base
    ports:
      - "80:80"
  worker:
    extends:
      file: base.yml
      service: base
    command: ["python", "worker.py"]
```

### 4. **Профили для разных окружений**
```yaml
services:
  webapp:
    # Всегда запускается
    image: nginx
    
  database:
    # Только с профилем "prod"
    profiles: ["prod"]
    image: postgres
    
  dev-tools:
    # Только с профилем "dev"
    profiles: ["dev"]
    image: node:18
    volumes:
      - ./src:/app
```

Запуск с профилями:
```bash
docker-compose --profile dev up  # Запустит webapp + dev-tools
docker-compose --profile prod up # Запустит webapp + database
```

---

## **Полный пример: Django + PostgreSQL + Redis + Celery**

```yaml
version: '3.8'

services:
  db:
    image: postgres:15
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      POSTGRES_DB: mydb
      POSTGRES_USER: user
      POSTGRES_PASSWORD: secret
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U user -d mydb"]
      interval: 10s
      timeout: 5s
      retries: 5

  redis:
    image: redis:7-alpine
    command: redis-server --appendonly yes
    volumes:
      - redis_data:/data

  web:
    build: .
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/app
      - static_volume:/app/static
    ports:
      - "8000:8000"
    environment:
      DATABASE_URL: postgresql://user:secret@db:5432/mydb
      REDIS_URL: redis://redis:6379/0
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_started

  celery:
    build: .
    command: celery -A myapp worker --loglevel=info
    volumes:
      - .:/app
    environment:
      DATABASE_URL: postgresql://user:secret@db:5432/mydb
      REDIS_URL: redis://redis:6379/0
    depends_on:
      - db
      - redis

  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
      - static_volume:/app/static
    depends_on:
      - web

volumes:
  postgres_data:
  redis_data:
  static_volume:

networks:
  default:
    name: myapp-network
```

---

## **Основные команды Docker Compose**

### Управление приложением
```bash
# Запустить все сервисы в фоне
docker-compose up -d

# Остановить все сервисы
docker-compose down

# Просмотр логов всех сервисов
docker-compose logs -f

# Просмотр логов конкретного сервиса
docker-compose logs -f webapp

# Перезапустить конкретный сервис
docker-compose restart webapp
```

### Управление сервисами
```bash
# Показать статус всех сервисов
docker-compose ps

# Выполнить команду в работающем контейнере
docker-compose exec database psql -U user mydb

# Пересобрать и перезапустить сервис
docker-compose up -d --build webapp

# Остановить только один сервис
docker-compose stop nginx
```

### Утилиты
```bash
# Показать все переменные окружения
docker-compose config

# Проверить синтаксис файла
docker-compose config --quiet

# Показать используемые порты
docker-compose port webapp 80

# Масштабирование сервиса (только без swarm)
docker-compose up -d --scale worker=3
```

### Восстановление данных
```bash
# Копировать файлы из/в контейнер
docker-compose cp db:/backup/data.sql ./restore/

# Создать бэкап базы данных
docker-compose exec db pg_dump -U user mydb > backup.sql

# Восстановить из бэкапа
cat backup.sql | docker-compose exec -T db psql -U user mydb
```

---

## **Docker Compose vs Docker Swarm vs Kubernetes**

| Особенность | Docker Compose | Docker Swarm Mode | Kubernetes |
|-------------|----------------|-------------------|------------|
| **Масштаб** | Одна машина | Несколько машин (кластер) | Крупные кластеры |
| **Сложность** | Низкая | Средняя | Высокая |
| **Файл конфига** | `docker-compose.yml` | `docker-compose.yml` (с расширениями) | Множество YAML-файлов |
| **Оркестрация** | Нет | Базовая | Полноценная |
| **Использование** | Локальная разработка | Небольшие продакшены | Крупные продакшены |

**Важно:** Современный Docker Compose (v2+) понимает некоторые Swarm-конфигурации, но для продакшена лучше использовать Kubernetes.

---

## **Лучшие практики**

### ✅ **Что делать:**
1. **Используйте `.env` файл** для конфиденциальных данных
2. **Определяйте healthchecks** для критичных сервисов
3. **Используйте именованные тома** для данных
4. **Настраивайте зависимости через `depends_on`**
5. **Логируйте в JSON** для парсинга системами мониторинга

### ❌ **Чего избегать:**
1. **Не хардкодьте пароли** в compose-файле
2. **Не используйте `latest` теги** в продакшене
3. **Не монтируйте весь код** в продакшене (только в разработке)
4. **Не игнорируйте ресурсные лимиты** на продакшене

---

## **Полезные трюки**

### 1. **Разные конфиги для разных окружений**
```bash
# docker-compose.prod.yml
# docker-compose.dev.yml

docker-compose -f docker-compose.yml -f docker-compose.override.yml up
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up
```

### 2. **Автоматическое обновление (для разработки)**
```yaml
services:
  web:
    build: .
    develop:
      watch:
        - action: rebuild
          path: .
          target: /app
```

### 3. **Conditional configuration**
```yaml
services:
  web:
    image: nginx
    ports:
      - "${WEB_PORT:-80}:80"
    deploy:
      replicas: ${REPLICAS:-1}
```

**Docker Compose — это мост между локальной разработкой и продакшеном.** Он позволяет вам определить инфраструктуру как код и использовать ее одинаково на всех этапах жизненного цикла приложения.

### **Часть 4: Dockerfile**

**Dockerfile** — это текстовый файл-инструкция, который описывает **как собрать Docker-образ**. Если образ — это готовое блюдо, то Dockerfile — его пошаговый рецепт.

### Базовый пример
```dockerfile
# Комментарий начинается с решетки

# 1. Базовый образ (обязательная первая инструкция)
FROM ubuntu:22.04

# 2. Метаданные (необязательно, но полезно)
LABEL maintainer="dev@example.com"
LABEL version="1.0"

# 3. Установка переменных окружения
ENV NODE_ENV=production
ENV APP_PORT=3000

# 4. Рабочая директория внутри контейнера
WORKDIR /app

# 5. Копируем файлы с хоста в контейнер
COPY package.json package-lock.json ./
COPY src ./src

# 6. Выполняем команды внутри контейнера (установка зависимостей)
RUN npm ci --only=production
RUN apt-get update && apt-get install -y curl

# 7. Открываем порт (информация для пользователя)
EXPOSE 3000

# 8. Точка входа или команда запуска (одна из двух)
CMD ["node", "src/server.js"]
# ИЛИ
# ENTRYPOINT ["node", "src/server.js"]
```

---

## **Ключевые инструкции (от самых важных к менее)**

### 1. **`FROM`** — фундамент
**Самая первая и обязательная инструкция.** Определяет базовый образ.
```dockerfile
FROM python:3.9-slim        # Официальный легкий Python
FROM node:18-alpine         # Alpine Linux — очень маленький образ
FROM nginx:latest           # Последняя версия nginx
FROM scratch                # Пустой образ (для супер-легких контейнеров)
```

### 2. **`RUN`** — выполнить команду
Выполняет команду **во время сборки образа** и сохраняет результат в слое.
```dockerfile
# Плохо: создает лишние слои и кэширует устаревшие списки пакетов
RUN apt-get update
RUN apt-get install -y python3
RUN pip install flask

# Хорошо: одна команда, один слой, очистка в том же слое
RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip \
    && pip install flask \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
```

### 3. **`COPY` vs `ADD`** — копирование файлов
**`COPY` (используйте всегда):** Просто копирует файлы/папки.
```dockerfile
COPY . /app                  # Копировать всё
COPY package.json ./         # Копировать конкретный файл
COPY src/ ./src/            # Копировать директорию
```

**`ADD` (используйте редко):** Умеет распаковывать архивы и скачивать URL.
```dockerfile
ADD https://example.com/file.tar.gz /tmp/  # Может скачать
ADD app.tar.gz /app/                       # Может распаковать
# Лучше избегать: непредсказуемо, менее прозрачно
```

### 4. **`CMD` vs `ENTRYPOINT`** — запуск контейнера

**`CMD`** — аргументы по умолчанию для запускаемого процесса.
```dockerfile
CMD ["nginx", "-g", "daemon off;"]  # JSON-формат (рекомендуется)
CMD nginx -g "daemon off;"          # Shell-формат
```
*Можно переопределить при запуске:* `docker run my-image /bin/bash`

**`ENTRYPOINT`** — фиксированная команда, которую не переопределить.
```dockerfile
ENTRYPOINT ["docker-entrypoint.sh"]
```
*CMD становится аргументами для ENTRYPOINT*

**Лучшая практика:** использовать вместе
```dockerfile
ENTRYPOINT ["python"]
CMD ["app.py"]  # По умолчанию запускаем app.py
# docker run my-image          → запустит python app.py
# docker run my-image test.py  → запустит python test.py
```

### 5. **`WORKDIR`** — рабочая директория
Устанавливает рабочую директорию для всех последующих инструкций.
```dockerfile
WORKDIR /app        # Абсолютный путь
WORKDIR src         # Относительный путь (от текущего WORKDIR)
RUN pwd             # Выведет /app/src
```

### 6. **`ENV`** — переменные окружения
```dockerfile
ENV NODE_ENV=production
ENV APP_HOME=/app APP_PORT=3000  # Можно несколько
```

### 7. **`ARG`** — переменные сборки
Доступны только во время сборки (не остаются в конечном образе).
```dockerfile
ARG VERSION=latest
FROM nginx:${VERSION}
```

### 8. **`.dockerignore`** — что НЕ копировать
**Обязательно создавайте этот файл!** Ускоряет сборку и уменьшает образ.
```
.git
node_modules
*.log
.env
Dockerfile
*.md
```

---

## **Практический пример: продвинутый Dockerfile для Node.js**
```dockerfile
# 1. Многоступенчатая сборка (multi-stage build)
# Этап 1: Сборка
FROM node:18-alpine AS builder
WORKDIR /build
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Этап 2: Продакшен
FROM node:18-alpine
WORKDIR /app

# 2. Безопасность: не root-пользователь
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001

# 3. Копируем только необходимое из этапа builder
COPY --from=builder --chown=nodejs:nodejs /build/dist ./dist
COPY --from=builder --chown=nodejs:nodejs /build/package*.json ./

# 4. Устанавливаем только production-зависимости
RUN npm ci --only=production

# 5. Переключаемся на непривилегированного пользователя
USER nodejs

# 6. Здоровье приложения
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD node healthcheck.js

# 7. Запуск
ENV NODE_ENV=production
EXPOSE 3000
CMD ["node", "dist/server.js"]
```

---

## **Лучшие практики**

### ✅ **Что делать:**
1. **Используйте `.dockerignore`** — уменьшает образ и ускоряет сборку
2. **Сортируйте инструкции** от реже изменяемых к чаще изменяемым:
   ```dockerfile
   # Сначала системные зависимости
   RUN apt-get update && apt-get install -y ...
   
   # Потом зависимости приложения
   COPY package.json ./
   RUN npm install
   
   # В конце код приложения
   COPY src/ ./src/
   ```
3. **Используйте multi-stage builds** для минимизации финального образа
4. **Указывайте конкретные теги версий** вместо `latest`
5. **Используйте непривилегированных пользователей** (`USER`)

### ❌ **Чего избегать:**
1. **Не храните секреты в образах** — используйте `docker run --env` или секреты
2. **Не устанавливайте лишнее** — каждый пакет увеличивает образ и поверхность атаки
3. **Не запускайте как root** — снижает риски безопасности
4. **Не копируйте всё подряд** — только необходимое для работы

---

## **Сборка и использование**
```bash
# Собрать образ из Dockerfile в текущей директории
docker build -t myapp:1.0 .

# Собрать с использованием кэша
docker build -t myapp:latest .

# Просмотреть историю сборки образа
docker history myapp:1.0

# Анализировать размеры слоев
docker image inspect myapp:1.0 --format='{{.RootFS.Layers}}'
```

**Dockerfile — это код инфраструктуры.** Хороший Dockerfile должен быть:
- **Минималистичным** (маленький образ)
- **Безопасным** (без root, без секретов)
- **Эффективным** (быстрая сборка благодаря кэшу)
- **Понятным** (ясные инструкции, комментарии)

Стоит писать его с той же тщательностью, что и бизнес-логику приложения.