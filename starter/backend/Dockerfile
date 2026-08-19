FROM python:3.10-slim

ENV FLASK_RUN_HOST=0.0.0.0

WORKDIR /

RUN adduser --uid 1000 --disabled-password app

RUN apt-get update && \
    apt-get install -y \
    build-essential \
    gcc \
    linux-libc-dev \
    libpcre3 \
    libpcre3-dev \
    libssl-dev \
    libffi-dev \
    && rm -rf /var/lib/apt/lists/*

RUN python -m pip install --upgrade pip setuptools wheel pipenv

RUN mkdir -p /var/www/app && mkdir /app

COPY Pipfile Pipfile.lock /app/

RUN cd /app && pipenv install --system --deploy

COPY . /app

CMD ["uwsgi", "--http", ":5000", "--master", "--enable-threads", "-s", "/var/www/app/app.sock", "--manage-script-name", "--mount", "/=app:app"]
