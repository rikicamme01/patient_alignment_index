
FROM python:3.11-slim

# Imposta le variabili d'ambiente
ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1 \
    PIP_NO_CACHE_DIR=1 \
    PIP_DISABLE_PIP_VERSION_CHECK=1

# Imposta la directory di lavoro
WORKDIR /app

# Installa le dipendenze di sistema
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*

# Copia il file dei requirements
COPY requirements.txt .

# Installa le dipendenze Python
RUN pip install --upgrade pip && \
    pip install -r requirements.txt

# Copia il progetto
COPY . .

# Raccoglie i file statici
RUN python manage.py collectstatic --noinput || true

# Espone la porta
EXPOSE 8000

# Comando per avviare l'applicazione
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "--workers", "4", "config.wsgi:application"]
