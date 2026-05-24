# 🤖 ego-ragcache: RAG-ассистент с ChromaDB, кешированием и ProxyAPI

Минимальный RAG-ассистент, объединяющий локальный поиск (ChromaDB), умное кеширование и интеграцию с **ProxyAPI** для работы в РФ без VPN. Форк проекта [MrGAN12009/prompt-5-7](https://github.com/MrGAN12009/prompt-5-7).

## 📋 Описание работы

Система работает по принципу:
1. **Проверка кеша** (JSON) → 2. **Векторный поиск** (ChromaDB) → 3. **Контекст** → 4. **LLM через ProxyAPI** → 5. **Ответ**.

## 🎯 Ключевые особенности форка

* 🇷🇺 **ProxyAPI** — работа с OpenAI из РФ без VPN (основной провайдер).
* ⚡ **Кеширование** — экономия API-запросов (SHA-256).
* 📦 **Локальная БД** — ChromaDB для семантического поиска.
* 🛠 **Готов к расширению** — структура под новые модули.

## 🔄 Что изменилось в этом форке

### Интеграция ProxyAPI

**Основное изменение:** Project теперь использует **ProxyAPI** по умолчанию для работы с OpenAI API из России без VPN.

**Технические изменения:**

1. **OpenAI клиент с base_url** — все запросы к OpenAI теперь отправляются через ProxyAPI endpoint:
   ```python
   client = OpenAI(
       api_key=os.getenv("OPENAI_API_KEY"),
       base_url="https://api.proxyapi.ru/openai/v1"
   )
   ```

2. **Поддержка альтернативных провайдеров** — можно переключиться на прямой OpenAI или другие совместимые API через переменную окружения `OPENAI_BASE_URL`.

3. **Изменённые модули:**
   - `main.py` — инициализация с конфигурацией ProxyAPI
   - `embeddings.py` — создание эмбеддингов через ProxyAPI
   - `rag.py` — генерация ответов через ProxyAPI

4. **Переменные окружения:**
   ```env
   OPENAI_API_KEY=your_proxyapi_key_here
   OPENAI_BASE_URL=https://api.proxyapi.ru/openai/v1
   ```

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

**Получить ключ ProxyAPI:** [proxyapi.ru](https://proxyapi.ru/)

### Настройка переменных окружения

| Переменная | Описание | Значение по умолчанию |
|------------|----------|----------------------|
| `OPENAI_API_KEY` | API ключ ProxyAPI | **Обязательно** |
| `OPENAI_BASE_URL` | Endpoint провайдера | `https://api.proxyapi.ru/openai/v1` |
| `TOP_K` | Количество документов для поиска | `3` |
| `SOURCE_FILTER` | Фильтр по источнику документов | `None` |
| `MODEL_NAME` | Модель для генерации | `gpt-3.5-turbo` |
| `TEMPERATURE` | Креативность (0.0-1.0) | `0.7` |
| `EMBEDDING_MODEL` | Модель для эмбеддингов | `text-embedding-3-small` |

**Альтернативные провайдеры:**
```env
# Прямой OpenAI (требуется VPN)
OPENAI_BASE_URL=https://api.openai.com/v1

# Другие совместимые endpoint'ы
# OPENAI_BASE_URL=https://your-custom-proxy/v1
```

## 🧪 Эксперименты с параметрами

### Изменение количества документов (TOP_K)

По умолчанию система использует 3 документа для контекста. Увеличьте это значение для более детальных ответов:

```env
TOP_K=7  # Увеличить количество документов
```

### Фильтрация по источнику

Ограничьте поиск документами из конкретного источника:

```env
SOURCE_FILTER=Python Основы
```

Доступные источники (из примеров):
- `Python Основы`
- `Машинное обучение и AI`
- `Векторные базы данных`

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

### Интеграция ProxyAPI

Инициализация клиента OpenAI с `base_url` для ProxyAPI:

```python
from openai import OpenAI

client = OpenAI(
    api_key=os.getenv("OPENAI_API_KEY"),
    base_url=os.getenv("OPENAI_BASE_URL", "https://api.proxyapi.ru/openai/v1")
)
```

**Изменённые модули:**

| Модуль | Изменения |
|--------|-----------|
| `main.py` | Добавлена конфигурация `base_url`, проверка `OPENAI_BASE_URL` |
| `embeddings.py` | `EmbeddingStore` принимает `base_url`, создаёт `OpenAI` клиент с endpoint'ом |
| `rag.py` | `RAGAssistant` принимает `base_url`, создаёт `OpenAI` клиент с endpoint'ом |

### Спецификации

*   **Модель эмбеддингов**: `text-embedding-3-small`
*   **Модель LLM**: `gpt-3.5-turbo` (настраивается)
*   **Чанкинг**: 500/50 (размер/перекрытие)
*   **Endpoint**: `https://api.proxyapi.ru/openai/v1`

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

