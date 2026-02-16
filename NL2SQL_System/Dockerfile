FROM python:3.11-slim

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    gcc \
    g++ \
    build-essential \
    libpq-dev \
    default-libmysqlclient-dev \
    pkg-config \
    curl \
    bash \
    && rm -rf /var/lib/apt/lists/*

# Copy and install Python dependencies first (for better caching)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Download spaCy English language model for PII detection
RUN python -m spacy download en_core_web_lg

# Copy application code
COPY . .

# Create logs directory
RUN mkdir -p logs

# Expose ports for both FastAPI and Streamlit
EXPOSE 8001 8501

# Health check for the backend service
HEALTHCHECK --interval=30s --timeout=10s --start-period=40s --retries=3 \
    CMD curl -f http://localhost:8001/health || exit 1

# Make start.sh executable
RUN chmod +x start.sh

# Default command
CMD ["bash", "start.sh"]