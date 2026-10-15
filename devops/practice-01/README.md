# Practice 01 — Docker: первый практикум

**Студент:** Malantse
**Группа:** ИТ-9.24.3-ДН

## Что сделано

- Запущен готовый сайт nginx в контейнере `lab-web` (порт 8080)
- Собран собственный образ `student-site:v1` на базе `nginx:stable-alpine`
- Собрана вторая версия `student-site:v2` (обновлён `index.html`)
- Запущены два независимых контейнера: `my-site` (8081) и `my-copy` (8082)
- Проверена независимая остановка контейнеров

## Файлы

- `Dockerfile` — инструкция сборки образа
- `index.html` — страница сайта
- `result.txt` — команды задания 7 и результат
- `kt_1_docker.docx` — отчёт по контрольной точке

## Команды

```bash
docker build -t student-site:v1 .
docker run -d --name my-site -p 127.0.0.1:8081:80 student-site:v1

docker build -t student-site:v2 .
docker run -d --name my-site -p 127.0.0.1:8081:80 student-site:v2
docker run -d --name my-copy -p 127.0.0.1:8082:80 student-site:v2
