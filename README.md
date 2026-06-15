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

```
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "-port", "8000"]
```

```
services:
    front:
        image: nginx:alpine
        volumes:
            - ./static/index.html:/usr/share/nginx/html/index.html
        depends_on:
            - back
    back:
        build: ./api
        environment:
            DB_HOST: ${DB_HOST}
            DB_USER: ${DB_USER}
            DB_PASSWORDD: ${DB_PASS}
            DB_NAME: ${DB_NAME}
        restart: always
        depends_on:
            - db
    db:
        image: mariadb
        restart: always
        environment:
            DB_HOST: ${DB_HOST}
            DB_USER: ${DB_USER}
            DB_PASSWORDD: ${DB_PASS}
            DB_NAME: ${DB_NAME}
        ports:
            - "3306:3306"
        volumes:
            - ./data:/var/lib/mysql
            - ./db_scripts:/docker-entrypoint-initdb.d

```
3. Запустить с помощью docker compose приложение

   Dockerfile backend

```
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]

```

config nginx

```
pstream back {
                server 192.168.1.17:8000;
        }
server {
        listen 80;
        server_name localhost;
        location / {
                root /usr/share/nginx/html;
                index index.html;
        }
        location /health {
                proxy_pass http://back/health;
        }
        location /docs {
                proxy_pass http://back/docs;
        }
        location /courses {
                proxy_pass http://back/courses;
        }
}

```

docker-compose

```
services:
    front:
        image: nginx:alpine
        volumes:
            - ./default.conf:/etc/nginx/conf.d/default.conf
            - ./static/index.html:/usr/share/nginx/html/index.html
        ports:
            - "80:80"
        depends_on:
            - back
    back:
        build: ./api
        environment:
            DB_HOST: ${DB_HOST}
            DB_USER: ${DB_USER}
            DB_PASSWORDD: ${DB_PASSWORD}
            DB_NAME: ${DB_NAME}
        restart: unless-stopped
        ports:
          - "8000:8000"
        depends_on:
            - db
    db:
        image: mariadb
        restart: always
        environment:
            MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
            MYSQL_HOST: ${DB_HOST}
            MYSQL_USER: ${DB_USER}
            MYSQL_PASSWORDD: ${DB_PASSWORD}
            MYSQL_NAME: ${DB_NAME}
        ports:
            - "3306:3306"
        volumes:
            - ./data:/var/lib/mysql
            - ./db_scripts:/docker-entrypoint-initdb.d

```

<img width="1339" height="827" alt="изображение" src="https://github.com/user-attachments/assets/a34288e9-47d3-459e-a5d3-a984bc36b336" />

