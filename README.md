# stock-analysis

# Задача

Разработать приложение для анализа информации по ликвидным российским акциям.

В приложении должно быть:

1) Возможность задать список акций, для закачки данных.
2) Механизм получения и сохранения котировок (OHLC) по заданным инструментам за 
произвольный период времени.
3) Веб-сервис, в котором будет реализован метод "get_summary", который вернет список 
всех сохраненных инструментов и процент(%) изменения цены для каждого инструмента за 
заданный промежуток времени.
4) Инструкция по запуску и использованию.

# Описание возможных решений задачи

## Источники данных

Получать данные будем через Tinkoff Инвестиции OpenAPI.

## Асинхронные фоновые запросы к источникам данных

Т.к. запросы к источникам данных могут занимать секунды и более, если данных много, 
то будем выполнять запросы асинхронно в фоновом режиме. Для простоты и скорости разработки
будем использовать django-background-tasks. Для лучшей горизонтальной масштабируемости, 
настраиваемости, гибкости лучше использовать celery. 
Для нотификации клиента о результатах выполнения фоновых задач можно было бы использовать 
веб-сокеты.

# Инструкция по запуску и использованию

To Be Done

# Ресурсы

```
---------------------------------------------------
получить список акций

POST /tasks/
{
    "action": "get_stocks_list"
    "stocks_tickers":  ["ALRS", "YNDX"]  # TODO: think about it
}
response:
{
    "id": <id>,
    "status": <status>
}

---------------------------------------------------
загрузить котировки акций

POST /tasks/
{
    "action": "get_stocks_quotes",
    "from": <from>,
    "to": <to>,
    "stocks_ticker": <ticker>
}
response:
{
    "id": <id>,
    "status": <status>
}

---------------------------------------------------
список доступных действий по загрузке данных

GET /tasks/available_actions/
response:
{
    ["get_stocks_list", "get_stocks_quotes"]
}

---------------------------------------------------
список загруженных акций

GET /stocks/
response:
{
    [...]
}

---------------------------------------------------
список загруженных котировок за заданный период

GET /stocks/<ticker>/quotes?from=<from>&to=<to>
response:
{
    [...]
}

---------------------------------------------------
получить сводную информацию по всем акциям за заданный период

GET /stocks/summary?from=<from>&to=<to>
response:
{
    [...]
}
```