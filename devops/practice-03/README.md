# Practice 3 — Production-Ready Docker

**Студент:** Маланцев
**Группа:** ИТ-9.24.3-ДН

## Что сделано

- **Уровень 1**: базовая упаковка FastAPI в `python:3.11`. Образ `secure-api:v1` — 420 МБ.
- **Уровень 2**: multi-stage build. Builder на `python:3.11-slim` + `build-essential` собирает wheels,
  runtime — чистый `python:3.11-slim` без компиляторов. `secure-api:v2` — **59 МБ**.
- **Уровень 3**: production-ready hardening:
  - non-root пользователь `appuser`
  - `--read-only` FS + `VOLUME /var/log/app` для логов
  - `ENV LOG_FILE=/var/log/app/app_logs.txt`
  - `HEALTHCHECK` каждые 30 секунд
  - exec-form `CMD` — uvicorn = PID 1, корректный SIGTERM

## Запуск (production-ready)

```bash
docker run -d \
  --name secure-api \
  -p 8080:8080 \
  --read-only \
  --cap-drop ALL \
  --security-opt no-new-privileges \
  secure-api:v3
```

## Проверки

```bash
curl http://localhost:8080/health
# {"status":"ok"}

curl "http://localhost:8080/hash?password=test3"
# {"hash":"$2b$12$..."}

docker exec secure-api cat /var/log/app/app_logs.txt
# Generated hash for a user

docker exec secure-api whoami
# appuser

docker exec secure-api cat /proc/1/cmdline | tr '\0' ' '
# uvicorn main:app --host 0.0.0.0 --port 8080

docker inspect --format='{{json .State.Health.Status}}' secure-api
# "healthy"
```

## Размеры образов

| Тег | Размер |
| v1  | 420 MB |
| v2  | 59  MB |
| v3  | 64  MB |
