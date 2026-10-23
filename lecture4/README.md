# Shop API + MySQL

## Запуск

Из каталога проекта:

```bash
docker compose up --build -d
docker compose ps
```

API: http://localhost:8080  
Swagger: http://localhost:8080/docs  
Prometheus: http://localhost:9090  
Grafana: http://localhost:3000  
Метрики FastAPI: http://localhost:8080/metrics

MySQL хранит данные в named volume `mysql_data`, поэтому они сохраняются после перезапуска контейнеров.
Для полного удаления данных: `docker compose down -v` (удалит БД!).

## Проверка API

```bash
curl -X POST http://localhost:8080/item \
  -H 'Content-Type: application/json' \
  -d '{"name":"Keyboard","price":49.9}'

curl -X POST http://localhost:8080/cart
curl -X POST http://localhost:8080/cart/1/add/1
curl http://localhost:8080/cart/1
```

Для локального запуска без Docker нужно задать `DATABASE_URL`, например:
`mysql+pymysql://shop:shop_password@127.0.0.1:3306/shop`.

## Изоляция транзакций

Сценарии для dirty read, non-repeatable read и phantom read находятся в `tests`.
Они специально выполняются в двух независимых MySQL-соединениях, чтобы порядок операций был наглядным.
