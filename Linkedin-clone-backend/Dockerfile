FROM python:3.9-slim

WORKDIR /app

COPY app.py.txt app.py
COPY models.py.txt models.py
COPY requirements.txt.txt requirements.txt

RUN pip install --no-cache-dir -r requirements.txt

RUN python models.py

EXPOSE 5000

CMD ["python", "app.py"]
