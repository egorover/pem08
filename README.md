# 🔍 AI-Анализатор рынка онлайн-кинотеатров

MVP приложение для анализа конкурентной среды в нише онлайн-кинотеатров с поддержкой мультимодальности (текст и изображения).

![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)
![FastAPI](https://img.shields.io/badge/FastAPI-0.104+-green.svg)
![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4o-purple.svg)

## 📋 Описание

Приложение предназначено для анализа конкурентов в сегменте онлайн-кинотеатров (Кинопоиск, Иви, Wink и др.). Позволяет:

- **Анализировать текст конкурентов** — получать структурированную аналитику с сильными/слабыми сторонами, уникальными предложениями и рекомендациями
- **Анализировать изображения** — баннеры, интерфейсы сайтов, промо-материалы с оценкой визуального стиля и потенциала для анимации
- **Парсить сайты** — автоматически извлекать и анализировать контент по URL
- **Оценивать каталог** — оценку разнообразия каталога контента (catalog_variety: 1-10)
- **Оценивать потенциал анимации** — анализ визуального дизайна для использования в соцсетях (animation_potential: 1-10)
- **Массовый парсинг** — одновременный анализ нескольких конкурентов
- **Хранить историю** — последние 10 запросов сохраняются для быстрого доступа

## 🚀 Быстрый старт

### 1. Клонирование и установка зависимостей

```bash
# Клонируйте репозиторий
cd competitor-monitor

# Создайте виртуальное окружение
python -m venv venv

# Активируйте окружение
# Windows:
venv\Scripts\activate
# Linux/Mac:
source venv/bin/activate

# Установите зависимости
pip install -r requirements.txt
```

### 2. Настройка переменных окружения

Создайте файл `.env` в корне проекта (используйте `env.example.txt` как шаблон):

```env
OPENAI_API_KEY=your_openai_api_key_here
OPENAI_MODEL=gpt-4o-mini
OPENAI_VISION_MODEL=gpt-4o-mini
```

### 3. Запуск приложения

```bash
# Запуск сервера
python -m uvicorn backend.main:app --reload --host 0.0.0.0 --port 8000
```

Приложение будет доступно по адресу: http://localhost:8000

## 📁 Структура проекта

```
competitor-monitor/
├── backend/
│   ├── __init__.py
│   ├── main.py              # FastAPI приложение
│   ├── config.py            # Конфигурация
│   ├── models/
│   │   ├── __init__.py
│   │   └── schemas.py       # Pydantic модели
│   └── services/
│       ├── __init__.py
│       ├── openai_service.py    # Работа с OpenAI API
│       ├── parser_service.py    # Парсинг веб-страниц
│       └── history_service.py   # Управление историей
├── frontend/
│   ├── index.html           # HTML страница
│   ├── styles.css           # Стили
│   └── app.js               # JavaScript логика
├── requirements.txt         # Зависимости Python
├── env.example.txt          # Пример .env файла
├── history.json             # Файл истории (создаётся автоматически)
├── README.md                # Этот файл
└── docs.md                  # Документация API
```

## 🔧 Функциональность

### Анализ текста (`/analyze_text`)
- Принимает текст конкурента (минимум 10 символов)
- Возвращает:
  - Сильные стороны
  - Слабые стороны
  - Уникальные предложения
  - Рекомендации по улучшению
  - Общее резюме
  - **catalog_variety**: оценка разнообразия каталога (1-10)

### Анализ изображений (`/analyze_image`)
- Принимает изображения: PNG, JPG, GIF, WEBP
- Возвращает:
  - Описание изображения
  - Маркетинговые инсайты
  - Оценку визуального стиля (0-10)
  - Рекомендации
  - **animation_potential**: потенциал для анимации в соцсетях (0-10)

### Парсинг сайтов (`/parse_demo`)
- Принимает URL сайта
- Извлекает: title, h1, первый абзац, скриншот
- Автоматически анализирует извлечённый контент через Vision API

### Массовый парсинг (`/parse_all_competitors`)
- Анализирует всех конкурентов из списка в `.env` (COMPETITORS_URLS)
- Возвращает комплексный анализ каждого сайта
- Открывает Chrome, делает скриншоты и анализирует через Vision API

### История (`/history`)
- Хранит последние 10 запросов
- Сохраняет тип запроса, краткое описание, время

## 🖥️ Desktop Приложение

В проекте присутствует модуль desktop/ для создания standalone .exe приложения на PyQt6.

### Сборка .exe файла

```bash
# Перейдите в папку desktop
cd desktop

# Установите зависимости
pip install pyinstaller PyQt6 requests

# Соберите .exe файл
python build.py
```

Готовый файл `CompetitorMonitor.exe` появится в папке `desktop/dist/`.

### Запуск Desktop приложения

⚠️ **Важно:** Desktop приложение работает только при запущенном бэкенде!

1. Запустите backend:
   ```bash
   python run.py
   ```
   или
   ```bash
   python -m uvicorn backend.main:app --reload --host 0.0.0.0 --port 8000
   ```

2. Запустите `CompetitorMonitor.exe`

Desktop приложение предоставляет тот же функционал, что и веб-интерфейс, но в виде нативного Windows приложения.

## 🛠️ Технологии

**Backend**
- FastAPI, Python 3.9+
- OpenAI GPT-4o-mini / GPT-4.1 (через ProxyAPI)
- BeautifulSoup4, httpx (парсинг)
- Pydantic (валидация)
- Selenium (скриншоты сайтов)

**Desktop**
- PyQt6 (GUI)
- PyInstaller (сборка .exe)

**Frontend**
- Vanilla JS, CSS3
- HTML5

## 📖 API Документация

После запуска сервера доступна интерактивная документация:
- Swagger UI: http://localhost:8000/docs
- ReDoc: http://localhost:8000/redoc

Подробная документация API в файле [docs.md](docs.md)

## ⚠️ Требования

- Python 3.9+
- ProxyAPI ключ (OpenAI-совместимый API для России)
- Интернет-соединение для работы AI и парсинга
- Chrome/Chromium браузер для парсинга сайтов
- Для Desktop приложения: PyQt6 и PyInstaller

## 📤 Деплой на GitHub

### Публикация репозитория

1. Создайте репозиторий на GitHub (приватный или публичный)

2. Инициализируйте Git (если ещё не инициализирован):
   ```bash
   git init
   ```

3. Добавьте файлы в Git:
   ```bash
   git add .
   git commit -m "feat: Добавлен анализатор для ниши онлайн-кинотеатров"
   ```

4. Привяжите к удалённому репозиторию и запушьте:
   ```bash
   git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
   git push -u origin main
   ```

### Публикация Desktop приложения

После сборки .exe файла (см. раздел "Desktop Приложение"):

1. Соберите .exe файл:
   ```bash
   cd desktop
   python build.py
   ```

2. Перейдите в корень проекта и создайте релиз на GitHub:
   - Зайдите в репозиторий на GitHub
   - Перейдите в раздел "Releases" → "Draft a new release"
   - Нажмите "Choose a file" и выберите `desktop/dist/CompetitorMonitor.exe`
   - Добавьте описание релиза и опубликуйте

### Важно о безопасности

⚠️ **Никогда не коммитьте файл `.env` с вашими API ключами!**

Файл `.env` уже добавлен в `.gitignore`. В репозитории должен остаться только `.env.example` с примерами конфигурации.

## 📝 Лицензия

MIT License

