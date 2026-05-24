# 🤖 ego-ragcache: RAG-ассистент с ChromaDB, кешированием и ProxyAPI

Минимальный RAG-ассистент, объединяющий локальный поиск (ChromaDB), умное кеширование и интеграцию с **ProxyAPI** для работы в РФ без VPN. Форк проекта [MrGAN12009/prompt-5-7](https://github.com/MrGAN12009/prompt-5-7).

## 📋 Описание работы

Система работает по принципу:
1. **Проверка кеша** (JSON) → 2. **Векторный поиск** (ChromaDB) → 3. **Контекст** → 4. **LLM через ProxyAPI** → 5. **Ответ**.

## 🎯 Ключевые особенности форка

* 🇷🇺 **ProxyAPI** — работа с OpenAI из РФ без VPN.
* ⚡ **Кеширование** — экономия API-запросов (SHA-256).
* 📦 **Локальная БД** — ChromaDB для семантического поиска.
* 🛠 **Готов к расширению** — структура под новые модули.

## 🎯 Ключевые особенности форка

* 🇷🇺 **ProxyAPI** — работа с OpenAI из РФ без VPN.
* ⚡ **Кеширование** — экономия API-запросов (SHA-256).
* 📦 **Локальная БД** — ChromaDB для семантического поиска.
* 🛠 **Готов к расширению** — структура под новые модули.

## 📦 Быстрый старт

1. **Клон**: `git clone https://github.com/egorover/ego-ragcache`
2. **Венв**: `python -m venv .venv`
3. **Активация**: `.\.venv\Scripts\Activate.ps1` (Windows) или `source .venv/bin/activate` (Linux/Mac)
4. **Установка**: `pip install -r requirements.txt`
5. **Конфиг**: Создайте `.env`:

```env
OPENAI_API_KEY=your_proxyapi_key_here
OPENAI_BASE_URL=https://api.proxyapi.ru/openai/v1
```

Получить ключ: [proxyapi.ru](https://proxyapi.ru/).

## 🚀 Запуск и Структура

```bash
python main.py
```

**Структура проекта:**
*   `main.py` — основной скрипт
*   `rag.py` — RAG логика
*   `cache.py` — JSON кеш
*   `embeddings.py` — ChromaDB

## 🔧 Архитектурные детали

Инициализация клиента OpenAI использует `base_url` для ProxyAPI:

```python
client = OpenAI(
    api_key=os.getenv("OPENAI_API_KEY"),
    base_url=os.getenv("OPENAI_BASE_URL")
)
```

**Спецификации:**
*   Модель: `text-embedding-3-small`
*   Чанкинг: 500/50

## 🛠 Roadmap

- [ ] Интеграция Claude/DeepSeek через ProxyAPI
- [ ] Авто-парсер документов (PDF/MD)

## 🤝 Контакты

PR и Issues приветствуются в репозитории [ego-ragcache](https://github.com/egorover/ego-ragcache).

## 📊 Технические детали

### Эмбеддинги

- **Модель**: text-embedding-3-small (OpenAI)
- **Размерность**: 1536
- **Языки**: поддержка множества языков, включая русский

### Чанкинг

- **Размер чанка**: 500 символов
- **Перекрытие**: 50 символов

### ChromaDB

- **Тип**: персистентное хранилище
- **Метрика**: cosine similarity
- **Расположение**: ./chroma_db/

### Кеш

- **Тип**: JSON файл
- **Ключ**: SHA-256 хеш нормализованного запроса
- **Расположение**: ./cache.json

## 🛠 Roadmap

- [ ] Интеграция Claude/DeepSeek через ProxyAPI
- [ ] Авто-парсер документов (PDF/MD)

## 🤝 Контакты

PR и Issues приветствуются в репозитории [ego-ragcache](https://github.com/egorover/ego-ragcache).

## 📝 Лицензия

Учебный проект, свободен для использования и модификации.

---

**Приятного использования! 🚀**

