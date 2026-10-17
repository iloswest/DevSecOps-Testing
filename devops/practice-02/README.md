# Practice 02 — WordPress + MariaDB (Docker Compose)

**Студент:** Маланцев
**Группа:** ИТ-9.24.3-ДН

## Что сделано

- Запущен блог из двух сервисов: WordPress + MariaDB через `docker compose`
- Создана и опубликована запись «Моя практика Docker 2»
- Проверено сохранение данных при `docker compose down` / `up -d`
- Восстановлена работа сайта после остановки базы данных

## Файлы

- `compose.yaml` — описание сервисов и томов
- `result.txt` — отчёт и результаты проверки
- `containers-before.txt`, `containers-after.txt` — ID контейнеров до/после пересоздания

## Запуск

```bash
docker compose up -d
```

Сайт: http://localhost:8090

## Остановка (данные сохраняются)

```bash
docker compose down      # без -v
```
