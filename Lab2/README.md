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


ENTRYPOINT ["python3", "route.py"]
```



## 3. Build Docker Image

```bash
docker build -t iti-flask-lab2 .
```

- `-t flask-app-img` sets the image name.

---

## 4. Run Container with Memory Limit and Port Mapping

```bash
docker run -d --name flask-app-container \
  --memory="100m" \
  -p 127.0.0.1:80:5000 \
  iti-flask-app-img
```

- `--memory="100m"` → limits container memory to 100MB.
- `-p 127.0.0.1:80:5000` → maps container port 5000 to host port 80.

---

## 5. Test Locally

Open browser at [http://127.0.0.1](http://127.0.0.1) and verify the Flask app is running.

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


- Verified locally and pushed image to Docker Hub.

