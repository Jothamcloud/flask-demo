# Stage 1: Build stage
FROM python:3.10-alpine as builder

WORKDIR /app

# Install build dependencies
RUN apk add --no-cache gcc musl-dev libffi-dev openssl-dev

# Install Python dependencies in a separate install directory
COPY /requirements.txt .
RUN pip install --no-cache-dir --prefix=/install -r requirements.txt

# Stage 2: Runtime stage
FROM python:3.10-alpine

WORKDIR /app

# Copy only the necessary runtime files from the build stage
COPY --from=builder /install /usr/local

# Copy application files
COPY src/ .

EXPOSE 5000

CMD ["python", "app.py"]