# 🐳 Lab #3 — Docker Registry & Compose

A hands-on lab covering private Docker registries, custom images, and Docker Compose multi-service setups.

---

## 📋 Table of Contents

- [Part 1 — Insecure Docker Registry](#part-1--insecure-docker-registry)
- [Part 2 — WordPress & MySQL with Docker Compose](#part-2--wordpress--mysql-with-docker-compose)
- [Part 3 — Bonus: Flask App + Nginx](#part-3--bonus-flask-app--nginx)

---

## Part 1 — Insecure Docker Registry

> Run a private insecure Docker registry using the CNCF distribution image.

### Steps

**1. Run the private registry**
```bash
docker run -d -p 5000:5000 --name registry registry:2
```

**2. Configure Docker to allow insecure registry**

Edit `/etc/docker/daemon.json`:
```json
{
  "insecure-registries": ["<YOUR_SERVER_IP>:5000"]
}
```
Then restart Docker:
```bash
sudo systemctl restart docker
```

**3. Build a custom Nginx image based on Alpine**

`Dockerfile`:
```dockerfile
FROM alpine:latest
RUN apk add --no-cache nginx
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

```bash
docker build -t my-nginx .
```

**4. Tag and push the image to the private registry**
```bash
docker tag my-nginx <YOUR_SERVER_IP>:5000/my-nginx
docker push <YOUR_SERVER_IP>:5000/my-nginx
```
![](https://github.com/MohamedElSayed215/ITI-Docker-K8s/blob/main/Lab3/Screenshots/private-registry.jpg)
![](https://github.com/MohamedElSayed215/ITI-Docker-K8s/blob/main/Lab3/Screenshots/volume-private-registry.jpg)

📖 Reference: [Docker Insecure Registry Docs](https://docs.docker.com/registry/insecure/)

---

## Part 2 — WordPress & MySQL with Docker Compose

> Run WordPress with a MySQL 5.7 database using Docker Compose, exposed on port 8080.

### `docker-compose.yml`

```yaml
version: '3.7'
services:
  wordpress-service:
    container_name: wordpress-container
    image: 'wordpress:latest'
    networks:
      - my-network
    ports:
    - 8080:80  
  mysql-service:
    container_name: mysql-container
    image: 'mysql:5.7'
    networks:
      - my-network
    volumes:
      - my-sql-db:/var/lib/mysql 
networks:
   my-network:

volumes:
   my-sql-db:
```
![](https://github.com/MohamedElSayed215/ITI-Docker-K8s/blob/main/Lab3/Screenshots/docker-compose-outputs.jpg)
### Run it

```bash
docker compose up -d
```

Access WordPress at: **http://localhost:8080**

---

## Part 3 — Bonus: Flask App + Nginx (10 pts)

> Push the Flask app image from Lab 2 to the private registry, run it via Docker Compose, and put Nginx in front of it on port 80.
![](https://github.com/MohamedElSayed215/ITI-Docker-K8s/blob/main/Lab3/Screenshots/flask-app-docker-compose.jpg)
### Steps

**1. Tag and push the Flask image to the private registry**
```bash
docker tag flask-app <YOUR_SERVER_IP>:5000/flask-app
docker push <YOUR_SERVER_IP>:5000/flask-app
```
![](https://github.com/MohamedElSayed215/ITI-Docker-K8s/blob/main/Lab3/Screenshots/volume-private-registry-flask-app.jpg)
**2. Docker Compose with Flask + Nginx**

`docker-compose.yml`:
```yaml
version: '3.7'

services:

  flask-app:
    container_name: flask-app-container
    image: 172.20.107.129:5000/flask-app-img:latest
    expose:
      - "5000"
    networks:
      - app-network

  nginx:
    container_name: nginx-container
    image: nginx:latest
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - flask-app
    networks:
      - app-network

networks:
  app-network: 
```

`nginx.conf`:
```nginx
server {
    listen 80;

    location / {
        proxy_pass http://flask-app:5000;
       # proxy_set_header Host $host;
       # proxy_set_header X-Real-IP $remote_addr;
    }
}
```
![](https://github.com/MohamedElSayed215/ITI-Docker-K8s/blob/main/Lab3/Screenshots/nginx-flask.jpg)
**3. Run everything**
```bash
docker compose up -d
```

- Flask App: **http://localhost:8080**
- Via Nginx: **http://localhost:80**

---

## ✅ Checklist

| Task | Status |
|------|--------|
| Run insecure private registry | ✅ |
| Build Nginx image from Alpine | ✅ |
| Push Nginx image to registry | ✅ |
| Docker Compose — WordPress + MySQL | ✅ |
| MySQL data mounted to `/var/lib/mysql` | ✅ |
| WordPress exposed on port 8080 | ✅ |
| Push Flask image to private registry | ✅ |
| Docker Compose — Flask from private registry | ✅ |
| Flask exposed on port 8080 | ✅ |
| Nginx reverse proxy on port 80 | ✅ |
