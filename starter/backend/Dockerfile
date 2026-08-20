FROM python:3.10-slim-bookworm

ENV FLASK_RUN_HOST=0.0.0.0

WORKDIR /app

RUN apt-get update && apt-get install -y \
    build-essential \
    gcc \
    libssl-dev \
    libffi-dev \
    && rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir pipenv

COPY Pipfile* /app/

RUN pipenv install --system --skip-lock

COPY . /app

EXPOSE 5000

CMD ["flask", "--app", ".", "run", "--host", "0.0.0.0", "--port", "5000"]