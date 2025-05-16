FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt /app/

RUN pip install -r requirements.txt

RUN apt-get update && apt-get install -y curl

COPY . /app

CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "5000"]
