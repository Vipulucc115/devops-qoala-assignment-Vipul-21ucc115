# DevOps Internship Challenge

This repository contains the solution for the DevOps Internship Challenge assignment. The project consists of a Dockerized application with an Nginx web server and a Python Flask backend. This README explains the issues found in the initial setup, debugging steps, and the changes made to ensure the application runs correctly.

This is written by MCP

## Project Structure

```
.
├── nginx/
│   ├── Dockerfile
│   └── nginx.conf
├── Python/
│   ├── app.py
│   └── Dockerfile
├── docker-compose.yml
└── README.md
```

## Issues Identified and Resolved

The following issues were identified and resolved during the debugging process.

### 1. Nginx Dockerfile Issues

#### Problem

The initial `nginx/Dockerfile` had the following issues:

- The image `nginx:latests` was misspelled. It should be `nginx:latest`.
- Incorrect paths in the `COPY` commands.
- The `EXPOSE` command used a string ("eighty") instead of the numeric port.
- `CMD` command had a typo (`daemon of;`).

#### Solution

The corrected `nginx/Dockerfile` is:

```dockerfile
FROM nginx:latest

COPY nginx.conf /etc/nginx/nginx.conf
COPY ./html /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

#### Explanation

- Fixed `nginx:latests` to `nginx:latest`: The correct image name allows Docker to pull the Nginx image.
- Fixed `COPY` commands: Corrected paths to avoid missing file errors.
- Updated `EXPOSE`: Changed "eighty" to the numeric port 80.
- Corrected `CMD`: Changed `daemon of;` to `daemon off;`, enabling Nginx to run in the foreground.

### 2. Nginx Configuration File (nginx.conf) Issues

#### Problem

The `nginx.conf` file had the following issues:

- Misspelled directives, such as `worker_process` instead of `worker_processes`.
- Incorrect path `mime.typess` for the MIME types configuration file.
- Misconfigured service name for proxy pass (`python_app` instead of `python-app`).

#### Solution

The corrected `nginx.conf` is:

```nginx
worker_processes auto;

events {
    worker_connections 1024;
}

http {
    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    server {
        listen 80;
        server_name localhost;

        location / {
            proxy_pass http://python-app:8000;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
}
```

#### Explanation

- Fixed typos in directives: Corrected `worker_process` and `mime.typess`.
- Updated `proxy_pass` target: Used `python-app` to match the container name specified in `docker-compose.yml`.

### 3. Python Dockerfile Issues

#### Problem

The initial `Python/Dockerfile` had several issues:

- Misspelled `WORKDIR` and file path in the `COPY` command.
- `EXPOSE` command used a string instead of a numeric port.
- The `CMD` command had typos in `python` and `app.py`.

#### Solution

The corrected `Python/Dockerfile` is:

```dockerfile
FROM python:3.9

WORKDIR /app

COPY app.py /app

RUN pip install flask netifaces

EXPOSE 8000

CMD ["python", "app.py"]
```

#### Explanation

- Fixed `WORKDIR` and `COPY` path: Set the working directory correctly and used the correct path for the application file.
- Updated `EXPOSE`: Changed "eight thousand" to 8000.
- Corrected `CMD` command: Fixed typos in `python` and `app.py`.

### 4. docker-compose.yml Issues

#### Problem

The initial `docker-compose.yml` had issues such as:

- Unnecessary `version` attribute that caused warnings.
- Incorrect port and volume names for the Nginx service.
- Typo in the container name (`python_app` vs. `python-app`).
- Incorrect `expose` value ("eight thousand").
- Typo in the driver and unnecessary options.

#### Solution

The corrected `docker-compose.yml` file is:

```yaml
services:
  nginx:
    build:
      context: ./nginx
      image: local-nginx
    ports:
      - "80:80"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
    networks:
      - nginx-network

  python-app:
    build:
      context: ./Python
      image: local-python-app
    container_name: python-app
    expose:
      - "8000"
    networks:
      - nginx-network

networks:
  nginx-network:
    driver: bridge
```