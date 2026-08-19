FROM python:3.10-slim

ENV FLASK_RUN_HOST=0.0.0.0

WORKDIR /app

# Install system dependencies required to build Python packages
RUN apt-get update && apt-get install -y \
    build-essential \
    gcc \
    g++ \
    python3-dev \
    libpcre3 \
    libpcre3-dev \
    libssl-dev \
    libffi-dev \
    && rm -rf /var/lib/apt/lists/*

# Install pipenv
RUN pip install --no-cache-dir pipenv

# Copy dependency files first
COPY Pipfile Pipfile.lock /app/

# Install Python dependencies
RUN pipenv install --system --deploy

# Copy application code
COPY . /app

# Create application user
RUN useradd -m -u 1000 app && \
    chown -R app:app /app

USER app

# Start uWSGI
CMD ["uwsgi", "--http", ":5000", "--master", "--enable-threads", "-s", "/tmp/app.sock", "--manage-script-name", "--mount", "/=app:app"]
