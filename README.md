# hw-22-docker-tms

1. Запустить Flask-приложение, сохраняющее логи в volume на хосте и маппингом портов. Структура проекта

```
FROM python:3.12-slim
WORKDIR /app
RUN pip install flask
COPY app.py .
CMD ["python3", "app.py"]
```

<img width="1180" height="503" alt="image" src="https://github.com/user-attachments/assets/43511ac9-8cc6-4154-8e9d-d1ad1da96f92" />

2. Запустить с помощью docker compose приложение https://github.com/AnastasiyaGapochkina01/simple-docker-apps/tree/main/py-http-server

```
#Dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY ./app/ .
CMD ["python3", "app.py"]
```

```
#docker-compose
services:
    py-server:
        build: ./app
        restart: always
        ports:
            - "8000:8000"
```

<img width="1312" height="663" alt="изображение" src="https://github.com/user-attachments/assets/69d1c36b-66c0-4150-a0de-9d3010ae8b1a" />

<img width="932" height="407" alt="изображение" src="https://github.com/user-attachments/assets/9ec56446-95d4-4fff-8ebf-dde27aa225eb" />
