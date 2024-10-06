# Stage 1: Build stage
FROM python:3.10-alpine as builder

WORKDIR /app

# Install build dependencies
RUN apk add --no-cache gcc musl-dev libffi-dev openssl-dev

# Upgrade pip and setuptools
RUN pip install --no-cache-dir --upgrade pip setuptools

# Copy requirements file from src directory
COPY src/requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir --prefix=/install -r requirements.txt

# Stage 2: Runtime stage
FROM python:3.10-alpine

WORKDIR /app

# Upgrade pip and setuptools in the final image as well
RUN pip install --no-cache-dir --upgrade pip setuptools

# Copy only the necessary runtime files from the builder stage
COPY --from=builder /install /usr/local

# Copy application files
COPY src/ .

# Expose the port the app runs on
EXPOSE 5000

# Command to run the application
CMD ["python", "app.py"]