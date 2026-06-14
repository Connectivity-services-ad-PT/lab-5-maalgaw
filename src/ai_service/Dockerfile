# syntax=docker/dockerfile:1.7

FROM python:3.11-slim

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

WORKDIR /service

# Cài dependencies
RUN pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir fastapi uvicorn[standard] pydantic requests

COPY main.py .

# Healthcheck
HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
  CMD python -c "import requests; import sys; sys.exit(0) if requests.get('http://localhost:9000/health').status_code == 200 else sys.exit(1)" || exit 1

EXPOSE 9000

CMD ["python", "main.py"]
