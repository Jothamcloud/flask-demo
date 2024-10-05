# Stage 1: Build stage
FROM python:3.10-alpine as builder

WORKDIR /app

# Install build dependencies
RUN apk add --no-cache gcc musl-dev libffi-dev openssl-dev

# List contents of the current directory
RUN ls -la

# Try to copy requirements.txt from multiple possible locations
COPY requirements.txt requirements.txt* ./
COPY src/requirements.txt src/requirements.txt* ./
COPY */requirements.txt */requirements.txt* ./

# List contents again to see if requirements.txt was copied
RUN ls -la

# Install Python dependencies in a separate install directory
RUN pip install --no-cache-dir --prefix=/install -r requirements.txt || echo "Failed to install requirements"

# Stage 2: Runtime stage
FROM python:3.10-alpine

WORKDIR /app

# Copy only the necessary runtime files from the build stage
COPY --from=builder /install /usr/local

# Copy application files
COPY src/ .

EXPOSE 5000

CMD ["python", "app.py"]