# Stage 1: Build dependencies
FROM python:3.11-slim AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Stage 2: Runtime image (Minimal & Secure)
FROM python:3.11-slim
WORKDIR /app

# Copy installed python packages from builder stage
COPY --from=builder /usr/local/lib/python3.11/site-packages /usr/local/lib/python3.11/site-packages
COPY main.py .

# SECURITY: Create and run as a non-root user
RUN useradd -u 10001 appuser
USER 10001

EXPOSE 8000
# Use python module execution to safely run uvicorn without binary path issues
CMD ["python", "-m", "uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]