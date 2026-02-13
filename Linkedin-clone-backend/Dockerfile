FROM python:3.9-slim

WORKDIR /app

# Copy all your .txt files but rename them to actual .py files during build
COPY app.py.txt app.py
COPY models.py.txt models.py
COPY requirements.txt.txt requirements.txt

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Run the models.py to create database
RUN python models.py

# Expose port
EXPOSE 5000

# Run the application
CMD ["python", "app.py"]