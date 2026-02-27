# Docker Fundamentals Lab: Resource Management & OOM Killer

## 📋 Overview
This lab demonstrates the practical application of **Docker Lab #1**, focusing on container lifecycle management, hardware resource constraints (CPU/Memory), and observing the Linux Kernel's behavior via **Control Groups (cgroups)** and the **Out-Of-Memory (OOM) Killer**.

---

## 🛠 Lab Tasks & Implementation

### 1. Basic Container Operations
* **Image Pulling**: Pulled the `nginx:alpine` image for a lightweight footprint.
* **Running Containers**: Started a container in detached mode with a custom name and port mapping:

```bash
docker run -d --name Mohamed-ITI-46 -p 8080:80 nginx:alpine
```
**Verification**: Successfully accessed the Nginx welcome page via `localhost:8080`.
![Verify](Lab1/screen-shots/verify.jpg) 

---

### 2. Resource Constraints (CPU & Memory)
Deployed a container with strict hardware limits:

* **Memory Limit**: 70MB  
* **CPU Limit**: 1 Core

To verify limits from within the container:

```bash
docker exec -it <container_id> sh
cat /sys/fs/cgroup/memory.max     # 73400320 bytes (70MB)
cat /sys/fs/cgroup/cpu.max        # 100000 100000 (1 CPU core)
cat /sys/fs/cgroup/memory.swap.events  # 0 swap activity
```
![](https://github.com/MohamedElSayed215/ITI-Docker-K8s/blob/main/Lab1/screen-shots/cpu_max.jpg) 

![](https://github.com/MohamedElSayed215/ITI-Docker-K8s/blob/main/Lab1/screen-shots/max_memory%20.jpg) 

![](https://github.com/MohamedElSayed215/ITI-Docker-K8s/blob/main/Lab1/screen-shots/no-swap.jpg) 

---

### ⚠️ The OOM Killer Challenge (Bonus)
Goal: Force a container to exceed its memory limit and observe system reaction.

**Step 1: Restricted Environment**

```bash
docker run -it --name memory-eater --memory="50m" --memory-swap="50m" alpine sh
```

**Step 2: Trigger Memory Exhaustion (Fork Bomb / Memory Bomb)**

Inside the container:

```bash
:(){ :|:& };:
```

**Step 3: Observation & Proof**

* **Process Termination**: Linux Kernel detected memory violation → OOM Killer triggered.  
* **Exit Code 137**: `docker ps -a` confirmed container exited with Status `(137)`.  
![]https://github.com/MohamedElSayed215/ITI-Docker-K8s/blob/main/Lab1/screen-shots/all-memory-ate.jpg) 

**Technical Significance**: Exit Code 137 = process killed by SIGKILL due to exceeding cgroup memory limit.

---

## 📂 Project Structure & Evidence
* **Container Lifecycle**: Terminal logs showing container creation & status.  
* **Resource Auditing**: Byte values captured from `/sys/fs/cgroup/`.  
* **Connectivity**: Verified Nginx server response.  
* **Crash Logs**: "Exited (137)" after OOM event.

---

## 💡 Key Learnings
* **Cgroups v2**: Docker uses Linux kernel features to isolate hardware resources.  
* **Hard Limits**: Setting `--memory-swap` equal to `--memory` ensures strict memory enforcement.  
* **Troubleshooting**: Exit Code 137 is a definitive sign of OOM events.

