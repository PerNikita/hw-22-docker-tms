# hw-22-docker-tms

1. Запустить Flask-приложение, сохраняющее логи в volume на хосте и маппингом портов. Структура проекта

```
FROM python:3.12-slim
WORKDIR /app
RUN pip install flask
COPY . .
CMD ["python3", "app.py"]
```

<img width="1180" height="503" alt="image" src="https://github.com/user-attachments/assets/43511ac9-8cc6-4154-8e9d-d1ad1da96f92" />
