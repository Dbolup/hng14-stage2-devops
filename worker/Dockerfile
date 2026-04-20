# Stage 1: Builder
FROM python:3.11-slim AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

# Stage 2: Final image
FROM python:3.11-slim
WORKDIR /app

# Create non-root user
RUN adduser --disabled-password --gecos '' appuser

# Copy installed packages from builder
COPY --from=builder /usr/local/lib/python3.11/site-packages /usr/local/lib/python3.11/site-packages

# Copy app code
COPY . .

RUN chown -R appuser:appuser /app
USER appuser

HEALTHCHECK --interval=30s --timeout=10s --start-period=10s --retries=3 \
  CMD python -c "import redis, os; r = redis.Redis(host=os.environ.get('REDIS_HOST','redis')); r.ping()" || exit 1

CMD ["python", "worker.py"]
