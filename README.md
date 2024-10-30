# DevOps Internship Challenge

This repository contains the solution for the DevOps Internship Challenge assignment. The project consists of a Dockerized application with an Nginx web server and a Python Flask backend. This README explains the issues found in the initial setup, debugging steps, and the changes made to ensure the application runs correctly.

## Project Structure

## Issues Identified and Resolved

The following issues were identified and resolved during the debugging process.

### 1. Nginx Dockerfile Issues

#### **Problem**

The initial `nginx/Dockerfile` had the following issues:

- The image `nginx:latests` was misspelled. It should be `nginx:latest`.
- Incorrect paths in the `COPY` commands.
- The `EXPOSE` command used a string ("eighty") instead of the numeric port.
- `CMD` command had a typo (`daemon of;`).

#### **Solution**

The corrected `nginx/Dockerfile` is:

```dockerfile
FROM nginx:latest

COPY nginx.conf /etc/nginx/nginx.conf
COPY ./html /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]

```
