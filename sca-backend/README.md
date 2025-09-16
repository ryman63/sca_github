# SCA Backend API - Backend API для облачной IDE

Backend приложение для облачной IDE с поддержкой статического анализа кода, написанное на Spring Boot.

## 🏗 Архитектура

### Технологический стек:
- **Spring Boot 3.2** - основной фреймворк
- **Spring Security** - аутентификация и авторизация
- **Spring WebSocket** - real-time коммуникация
- **Spring Data JPA** - работа с базой данных
- **PostgreSQL** - основная база данных
- **Redis** - кэширование и сессии
- **Docker** - контейнеризация и изоляция
- **SonarQube** - статический анализ кода

### Основные компоненты:
1. **User Management** - управление пользователями
2. **Project Management** - управление проектами
3. **Code Analysis** - статический анализ кода
4. **Docker Service** - управление контейнерами
5. **WebSocket** - real-time коммуникация
6. **GitHub Integration** - интеграция с GitHub

## 🚀 Быстрый старт

### Предварительные требования:
- Java 17+
- Maven 3.6+
- Docker & Docker Compose
- PostgreSQL 15+
- Redis 7+

### Локальная разработка:

1. **Клонируйте репозиторий:**
```bash
git clone <repository-url>
cd backend
```

2. **Настройте базу данных:**
```bash
# Запустите PostgreSQL и Redis
docker-compose up postgres redis -d
```

3. **Настройте переменные окружения:**
```bash
cp application.yml.example application.yml
# Отредактируйте application.yml
```

4. **Запустите приложение:**
```bash
mvn spring-boot:run
```

### Развертывание с Docker:

1. **Из корневой папки проекта:**
```bash
# Запуск всех сервисов
docker-compose up -d

# Или только backend и его зависимости
docker-compose up backend postgres redis sonarqube -d
```

2. **Проверьте статус:**
```bash
docker-compose ps
```

3. **Просмотрите логи:**
```bash
docker-compose logs -f backend
# Или для конкретного сервиса:
docker-compose logs -f sca-backend-api
```

## 📋 API Endpoints

### Аутентификация:
- `POST /api/auth/login` - вход в систему
- `POST /api/auth/register` - регистрация
- `POST /api/auth/refresh` - обновление токена

### Проекты:
- `GET /api/projects` - список проектов пользователя
- `POST /api/projects` - создание проекта
- `GET /api/projects/{id}` - получение проекта
- `PUT /api/projects/{id}` - обновление проекта
- `DELETE /api/projects/{id}` - удаление проекта
- `POST /api/projects/clone` - клонирование из GitHub

### Анализ кода:
- `POST /api/analysis/{projectId}` - запуск анализа
- `GET /api/analysis/{projectId}/results` - результаты анализа

### WebSocket endpoints:
- `/ws` - WebSocket endpoint
- `/app/analyze` - запрос на анализ
- `/app/problems` - получение проблем
- `/app/file/update` - обновление файла

## 🔧 Конфигурация

### Основные настройки (application.yml):

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/sca_ide
    username: postgres
    password: password
  
  redis:
    host: localhost
    port: 6379

docker:
  host: tcp://localhost:2375
  containers:
    memory-limit: 512m
    cpu-limit: 0.5

websocket:
  endpoint: /ws
  allowed-origins:
    - http://localhost:3000
    - https://diploma-works.github.io
```

### Переменные окружения:

```bash
# База данных
DB_USERNAME=postgres
DB_PASSWORD=password
DB_HOST=localhost

# Redis
REDIS_PASSWORD=

# JWT
JWT_SECRET=your-secret-key-here

# GitHub
GITHUB_TOKEN=your-github-token

# Docker
DOCKER_HOST=tcp://localhost:2375
```

## 🐳 Docker

### Сборка образа:
```bash
docker build -t sca-backend .
```

### Запуск с docker-compose:
```bash
# Из корневой папки проекта
# Все сервисы
docker-compose up -d

# Только backend и его зависимости
docker-compose up backend postgres redis sonarqube -d

# С пересборкой
docker-compose up --build
```

### Просмотр логов:
```bash
docker-compose logs -f backend
```

## 🔍 Анализ кода

### Поддерживаемые инструменты:
1. **SonarQube** - комплексный анализ
2. **PMD** - анализ Java кода
3. **Checkstyle** - проверка стиля кода
4. **SpotBugs** - поиск багов

### Процесс анализа:
1. Создание Docker контейнера для анализа
2. Копирование проекта в контейнер
3. Установка и запуск инструментов анализа
4. Парсинг результатов
5. Сохранение проблем в БД
6. Отправка результатов через WebSocket

## 🔐 Безопасность

### Аутентификация:
- JWT токены
- Spring Security
- BCrypt хеширование паролей

### Авторизация:
- Роли пользователей (USER, ADMIN)
- Проверка прав доступа к проектам

### Docker безопасность:
- Изоляция пользовательских окружений
- Ограничения ресурсов
- Автоматическая очистка контейнеров

## 📊 Мониторинг

### Health checks:
- `GET /actuator/health` - состояние приложения
- `GET /actuator/info` - информация о приложении
- `GET /actuator/metrics` - метрики

### Логирование:
- Структурированные логи
- Различные уровни логирования
- Ротация логов

## 🧪 Тестирование

### Запуск тестов:
```bash
# Все тесты
mvn test

# Только unit тесты
mvn test -Dtest=UnitTest

# Интеграционные тесты
mvn test -Dtest=IntegrationTest
```

### Тестовое покрытие:
```bash
mvn jacoco:report
```

## 📈 Производительность

### Оптимизации:
- Кэширование с Redis
- Асинхронная обработка
- Connection pooling
- Docker контейнеры с ограничениями

### Мониторинг:
- Spring Boot Actuator
- Micrometer метрики
- Docker статистика

## 🤝 Интеграция с Frontend

### WebSocket сообщения:
```javascript
// Подключение
const socket = new SockJS('/ws');
const stompClient = Stomp.over(socket);

// Отправка запроса на анализ
stompClient.send("/app/analyze", {}, {
    projectId: 1,
    user: currentUser
});

// Получение результатов
stompClient.subscribe('/user/queue/analysis-results', function(response) {
    const results = JSON.parse(response.body);
    // Обработка результатов
});
```

### REST API:
```javascript
// Получение проектов
fetch('/api/projects', {
    headers: {
        'Authorization': 'Bearer ' + token
    }
});

// Создание проекта
fetch('/api/projects', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer ' + token
    },
    body: JSON.stringify(project)
});
```

## 🚀 Развертывание в продакшене

### Рекомендации:
1. Используйте HTTPS
2. Настройте SSL сертификаты
3. Используйте внешнюю базу данных
4. Настройте мониторинг
5. Настройте backup стратегию
6. Используйте load balancer

### Команды для продакшена:
```bash
# Сборка production образа
docker build -t sca-backend:prod .

# Запуск с production конфигурацией
docker-compose -f docker-compose.prod.yml up -d
```

## 📝 Лицензия

MIT License 