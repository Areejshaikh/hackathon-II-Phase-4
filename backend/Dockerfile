# Use Python 3.11 slim image as base
FROM python:3.11-slim

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PYTHONPATH=/app

# Set work directory
WORKDIR /app

# Install system dependencies
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        build-essential \
        gcc \
    && rm -rf /var/lib/apt/lists/*

# Copy poetry lock file and pyproject.toml
COPY pyproject.toml .

# Install Python dependencies
RUN pip install --upgrade pip && \
    pip install "poetry==1.8.3" && \
    poetry config virtualenvs.create false && \
    poetry install --only main --no-interaction

# If requirements.txt exists, install those too (fallback)
COPY requirements.txt .
RUN pip install -r requirements.txt

# Copy project
COPY . .

# Expose port
EXPOSE 8000

# Health check endpoint
HEALTHCHECK --interval=30s --timeout=30s --start-period=5s --retries=3 \
    CMD curl -f http://localhost:8000/health || exit 1

# Run the application
CMD ["python", "-m", "uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]