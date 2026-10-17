# Stage 1: builder
FROM python:3.11-slim AS builder

WORKDIR /build

RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    libffi-dev \
    && rm -rf /var/lib/apt/lists/*

COPY requirements.txt .

RUN pip wheel --no-cache-dir --wheel-dir /wheels -r requirements.txt


# Stage 2: runtime
FROM python:3.11-slim AS runtime

# curl нужен для HEALTHCHECK
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Непривилегированный пользователь
RUN groupadd -r appuser && useradd -r -g appuser -d /app -s /sbin/nologin appuser

WORKDIR /app

COPY --from=builder /wheels /wheels
COPY requirements.txt .

RUN pip install --no-cache-dir --no-index --find-links=/wheels -r requirements.txt \
    && rm -rf /wheels

COPY main.py .

# Папка для логов + права для appuser
RUN mkdir -p /var/log/app && chown -R appuser:appuser /var/log/app

# Переменная окружения для пути логов
ENV LOG_FILE=/var/log/app/app_logs.txt

# VOLUME для логов — работает даже при --read-only
VOLUME ["/var/log/app"]

# Переключаемся на непривилегированного пользователя
USER appuser

EXPOSE 8080

# HEALTHCHECK каждые 30 секунд
HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
    CMD curl -fsS http://localhost:8080/health || exit 1

# Exec form — uvicorn = PID 1, получает SIGTERM
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8080"]
