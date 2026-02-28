# Lab 2 - Flask Docker Image

## Overview
In this lab, we build a Python Flask Docker image from a GitHub repository, run it with memory limit, map ports, and push the image to Docker Hub.

---

## 1. Clone the Repository

```bash
git clone https://github.com/meldafrawi/basic-flask-app.git
cd basic-flask-app
```

---

## 2. Dockerfile

```dockerfile
# Dockerfile
FROM alpine:latest

RUN apk update && apk add python3 py3-pip py3-flask



COPY . /app

WORKDIR /app

ENTRYPOINT [ "python3", "routes.py" ]

```



## 3. Build Docker Image

```bash
docker build -t flask-app-img .
```

- `-t flask-app-img` sets the image name.
![](https://github.com/MohamedElSayed215/ITI-Docker-K8s/blob/main/Lab2/screenshots/image.jpg)
---

## 4. Run Container with Memory Limit and Port Mapping

```bash
docker run -d --name flask-app-container \
  --memory="100m" \
  -p 127.0.0.1:80:5000 \
  iti-flask-app-img
```
![](https://github.com/MohamedElSayed215/ITI-Docker-K8s/blob/main/Lab2/screenshots/container.jpg)
- `--memory="100m"` → limits container memory to 100MB.
- `-p 127.0.0.1:80:5000` → maps container port 5000 to host port 80.

---

## 5. Test Locally

Open browser at 'localhost:80' and verify the Flask app is running.
![](https://github.com/MohamedElSayed215/ITI-Docker-K8s/blob/main/Lab2/screenshots/localhost.jpg)

---

## 6. Push to Docker Hub

1. Login to Docker Hub:

```bash
docker login
```

2. Tag the image:

```bash
docker tag iti-flask-lab2 mohamedelsayed215/flask-app-img:latest
```

3. Push the image:

```bash
docker push mohamedelsayed215/flask-app-img:latest
```

> Docker Hub repository: [https://hub.docker.com/repository/docker/mohamedelsayed215/flask-app-img](https://hub.docker.com/repository/docker/mohamedelsayed215/flask-app-img)


1. Create the Network

```bash
docker network create --driver bridge --subnet 10.0.0.0/8 iti_network
```

- **Network Name:** `iti_network`  
- **Driver:** `bridge`  
- **Subnet:** `10.0.0.0/8`

---

## 2. Prepare the HTML Page

Create a folder `lab2-site` containing `index.html`:

```html
<h1>Lab 2 ITI - Mohamed Elsayed</h1>
```


---

## 3. Run the Webserver Container

```bash
docker run -d \
  --name webserver-iti \
  --network iti_network \
  -p 8080:80 \
  -v $(pwd)/lab2-site/index.html:/usr/share/nginx/html/index.html:ro \
  nginx:alpine
```

**Options explained:**

- `-d` → run in detached mode  
- `--name webserver-iti` → container name  
- `--network iti_network` → attach container to the custom network  
- `-p 8080:80` → map container port 80 to host port 8080  
- `-v ...` → volume to mount the HTML page

> For httpd instead of nginx:

```bash
-v $(pwd)/index.html:/usr/local/apache2/htdocs/index.html:ro
```

---

## 4. Verify

![](https://github.com/MohamedElSayed215/ITI-Docker-K8s/blob/main/Lab2/screenshots/part2.jpg) 
You should see:

```html
<h1>Lab 2 ITI - Mohamed Elsayed</h1>
```

---
## 5. Manage Network & Container

- List networks:

```bash
docker network ls
```

- Inspect network:

```bash
docker network inspect iti_network
```
![](https://github.com/MohamedElSayed215/ITI-Docker-K8s/blob/main/Lab2/screenshots/network.jpg)

