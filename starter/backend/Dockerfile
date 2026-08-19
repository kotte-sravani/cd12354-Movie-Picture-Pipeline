FROM public.ecr.aws/docker/library/python:3.10-alpine

ENV FLASK_RUN_HOST=0.0.0.0

WORKDIR /app

RUN adduser -u 1000 -D app

RUN apk add --no-cache \
    pcre \
    pcre-dev \
    build-base \
    linux-headers \
    openssl \
    libffi-dev

RUN pip install --no-cache-dir pipenv

RUN mkdir -p /var/www/app

COPY Pipfile Pipfile.lock /app/

RUN pipenv install --system --deploy

COPY . /app

CMD ["uwsgi", "--http", ":5000", "--master", "--enable-threads", "-s", "/var/www/app/app.sock", "--manage-script-name", "--mount", "/=app:app"]