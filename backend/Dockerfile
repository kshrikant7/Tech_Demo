# Dockerfile for backend
FROM python:3.7

WORKDIR /app

COPY . /app

RUN pip install --no-cache-dir -r requirements.txt

ARG OPENAI_API_KEY
ENV OPENAI_API_KEY=$OPENAI_API_KEY
    
CMD ["python", "app.py"]