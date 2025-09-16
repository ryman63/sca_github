# SCA Frontend - Облачная IDE с анализом кода

Полнофункциональная облачная IDE с поддержкой статического анализа кода, построенная на современном технологическом стеке.

## 🎯 О проекте

SCA Frontend - это веб-приложение, которое предоставляет полноценную среду разработки в браузере с возможностью анализа кода и выявления потенциальных проблем. Проект состоит из frontend (React) и backend (Spring Boot) компонентов.

### Основные возможности:
- 📝 **Редактор кода** с подсветкой синтаксиса
- 🔍 **Статический анализ кода** с помощью различных инструментов
- 📊 **Статистика и метрики** проекта
- 🐛 **Выявление проблем** с описанием и решениями
- 🔗 **Интеграция с GitHub** для клонирования репозиториев
- 🐳 **Docker контейнеры** для изоляции окружений
- ⚡ **Real-time коммуникация** через WebSocket

## 🏗 Архитектура

### Frontend (React + TypeScript)
```
src/
├── components/          # React компоненты
│   ├── Editor/         # Редактор кода
│   ├── Problems/       # Отображение проблем
│   ├── Statistics/     # Статистика проекта
│   ├── ProjectStructure/ # Структура проекта
│   └── GitHub/         # GitHub интеграция
├── themes/             # Темы оформления
├── utils/              # Утилиты
└── hooks/              # React хуки
```

### Backend (Spring Boot + Java)
```
src/main/java/com/sca/
├── config/             # Конфигурации
├── controller/         # REST контроллеры
├── model/              # JPA модели
├── service/            # Бизнес-логика
├── repository/         # Data Access Layer
└── security/           # Безопасность
```

### Инфраструктура
- **PostgreSQL** - основная база данных
- **Redis** - кэширование и сессии
- **Docker** - контейнеризация
- **SonarQube** - анализ кода
- **Nginx** - проксирование

## 🚀 Быстрый старт

### Предварительные требования:
- Node.js 18+
- Java 17+
- Docker & Docker Compose
- Git

### 1. Клонирование репозитория:
```bash
git clone <repository-url>
cd sca-frontend
```

### 2. Настройка Frontend:
```bash
# Установка зависимостей
npm install

# Настройка переменных окружения
cp env.example .env
# Отредактируйте .env файл

# Запуск в режиме разработки
npm start
```

### 3. Настройка Backend:
```bash
cd sca-backend

# Локально
mvn spring-boot:run
```

### 4. Запуск всего проекта с Docker:
```bash
# Из корневой папки проекта
docker-compose up -d
```

### 4. Доступ к приложению:
- Frontend: http://localhost:3000
- Backend API: http://localhost:8080/api
- SonarQube: http://localhost:9000

## 📋 Функциональность

### 🔐 Аутентификация
- Регистрация и вход пользователей
- JWT токены для безопасности
- Роли пользователей (USER, ADMIN)

### 📁 Управление проектами
- Создание новых проектов
- Клонирование из GitHub
- Просмотр структуры файлов
- Редактирование файлов

### 🔍 Анализ кода
- **SonarQube** - комплексный анализ
- **PMD** - анализ Java кода
- **Checkstyle** - проверка стиля
- **SpotBugs** - поиск багов

### 📊 Статистика
- Анализ авторов проекта
- Выявление проблемных файлов
- Визуализация данных
- Метрики качества кода

### 🐳 Docker интеграция
- Изоляция пользовательских окружений
- Ограничения ресурсов
- Автоматическая очистка
- Безопасность контейнеров

## 🔧 Конфигурация

### Frontend (env.example):
```env
# API Configuration
REACT_APP_API_URL=http://localhost:8080/api
REACT_APP_WS_URL=ws://localhost:8080/ws

# GitHub Configuration
REACT_APP_GITHUB_TOKEN=your-github-token

# Development Configuration
REACT_APP_ENV=development
```

Скопируйте `env.example` в `.env` и настройте переменные окружения.

### Backend (application.yml):
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
```

## 🐳 Docker развертывание

### Полное развертывание:
```bash
# Из корневой папки проекта
docker-compose up -d
```

### Развертывание отдельных компонентов:
```bash
# Только frontend
docker-compose up frontend -d

# Только backend и его зависимости
docker-compose up backend postgres redis sonarqube -d

# Только инфраструктура
docker-compose up postgres redis sonarqube nginx -d
```

### Production развертывание:
```bash
# Сборка и запуск всех сервисов
docker-compose up -d --build

# Или с production тегами
docker-compose -f docker-compose.prod.yml up -d
```

## 📊 API документация

### REST API:
- `GET /api/projects` - список проектов
- `POST /api/projects` - создание проекта
- `GET /api/projects/{id}` - получение проекта
- `POST /api/analysis/{id}` - запуск анализа

### WebSocket API:
- `/ws` - WebSocket endpoint
- `/app/analyze` - запрос на анализ
- `/app/problems` - получение проблем
- `/app/file/update` - обновление файла

## 🔐 Безопасность

### Аутентификация:
- JWT токены
- Spring Security
- BCrypt хеширование

### Авторизация:
- Роли пользователей
- Проверка прав доступа
- Изоляция проектов

### Docker безопасность:
- Ограничения ресурсов
- Изоляция окружений
- Автоматическая очистка

## 📈 Производительность

### Оптимизации:
- Кэширование с Redis
- Асинхронная обработка
- Connection pooling
- Docker контейнеры

### Мониторинг:
- Spring Boot Actuator
- React DevTools
- Docker статистика

## 🧪 Тестирование

### Frontend тесты:
```bash
npm test
npm run test:coverage
```

### Backend тесты:
```bash
mvn test
mvn jacoco:report
```

### E2E тесты:
```bash
npm run test:e2e
```

## 🤝 Разработка

### Структура проекта:
```
sca-frontend/
├── frontend/           # React приложение
│   ├── src/
│   ├── public/
│   └── package.json
├── backend/            # Spring Boot приложение
│   ├── src/
│   ├── pom.xml
│   └── docker-compose.yml
└── docs/              # Документация
```

### Команды разработки:
```bash
# Frontend
npm start              # Запуск dev сервера
npm run build          # Сборка для продакшена
npm run test           # Запуск тестов

# Backend
mvn spring-boot:run    # Запуск приложения
mvn test               # Запуск тестов
mvn clean package      # Сборка JAR
```

## 🚀 Развертывание

### Локальная разработка:
1. Запустите PostgreSQL и Redis
2. Запустите backend: `mvn spring-boot:run`
3. Запустите frontend: `npm start`

### Docker развертывание:
1. Соберите образы: `docker-compose build`
2. Запустите сервисы: `docker-compose up -d`
3. Проверьте статус: `docker-compose ps`

### Production развертывание:
1. Настройте SSL сертификаты
2. Используйте внешнюю базу данных
3. Настройте мониторинг
4. Используйте load balancer

## 📝 Лицензия

MIT License

## 🤝 Вклад в проект

1. Fork репозитория
2. Создайте feature branch
3. Внесите изменения
4. Добавьте тесты
5. Создайте Pull Request

## 📞 Поддержка

- Issues: GitHub Issues
- Документация: `/docs`
- Email: support@sca-ide.com

---

**SCA Cloud IDE** - современная облачная IDE с анализом кода для разработчиков.