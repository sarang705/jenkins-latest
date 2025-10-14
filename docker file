# Use an official Python runtime
FROM python:3.9-slim

# Set working directory
WORKDIR /app

# Copy source code
COPY . .

# Install dependencies
RUN pip install --no-cache-dir flask

# Run the application
CMD ["python", "app.py"]
