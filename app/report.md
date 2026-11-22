
# Отчёт по Docker

## 1. Проверка сохранения счётчика

Выполнила три запроса:

```bash
curl -X POST http://127.0.0.1:8080/increment
curl -X POST http://127.0.0.1:8080/increment
curl -X POST http://127.0.0.1:8080/increment
```

Получила `N = 3`.

Затем выполнила:

```bash
sudo docker compose down
sudo docker compose up -d
curl http://127.0.0.1:8080/
```

Результат: `count = 3`

## 2. Проверка остановки Redis

```bash
sudo docker compose stop redis
curl -i http://127.0.0.1:8080/healthz
curl -i http://127.0.0.1:8080/readyz
curl -i -X POST http://127.0.0.1:8080/increment
```

Результаты:
- `GET /healthz` — 200
- `GET /readyz` — 503
- `POST /increment` — 503

## 3. Проверка восстановления Redis

```bash
sudo docker compose start redis
sudo docker compose ps
curl http://127.0.0.1:8080/
curl -X POST http://127.0.0.1:8080/increment
```

Результаты:
- `GET /` — `count = 3`
- `POST /increment` — `count = 4`