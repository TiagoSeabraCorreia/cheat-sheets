# 🐳 **Docker Command Reference**

**📅 Date:** Saturday, October 25, 2025  
**🕒 Time:** 15:57  

---

## 🚀 **Basic Example**

```bash
docker run hello-world
```

✅ Runs a simple test image to verify your Docker installation.

---

## 🧱 **Creating and Starting a Container**

```bash
docker create busybox ping google.com
docker start <container_id>
```

📝 **Notes:**
- `docker create` prepares a container **without starting it**.  
- `docker start` launches the container.  
- In this case, it continuously pings **google.com** inside the container.  
- To view running containers:
  ```bash
  docker ps
  ```

---

## 🧹 **Cleaning Up System Resources**

```bash
docker system prune
```

🧼 **Removes:**
- Stopped containers  
- Unused networks  
- Dangling images (untagged)  
- Build cache  

⚠️ Add `-a` to also remove **unused images**:
```bash
docker system prune -a
```

---

## 🧨 **Stopping vs Killing Containers**

| Command | Signal | Behavior |
|----------|---------|-----------|
| `docker stop` | `SIGTERM` | Gracefully stops the container |
| `docker kill` | `SIGKILL` | Immediately forces the container to shut down |

🧠 **Tip:**  
If a container doesn’t stop within **10 seconds**, Docker automatically sends `SIGKILL`.

---

## 💬 **Executing Commands Inside Containers**

```bash
docker exec -it <container_id> redis-cli
```

📘 **Explanation:**
- `-i` → keeps STDIN open (interactive input/output).  
- `-t` → allocates a pseudo-TTY (formats text properly).  
- Combined (`-it`) → allows **interactive terminal access** inside the container.

🔹 **Example:**
```bash
docker exec -it 6e895bf2e27c redis-cli
```

---

## 🧭 **Extra Tips**

- Attach to a container’s live output:
  ```bash
  docker attach <container_id>
  ```
- Run and remove the container after exit:
  ```bash
  docker run --rm <image_name>
  ```
- Give your container a custom name:
  ```bash
  docker run --name my-container <image_name>
  ```

---

> 💡 **Summary:**  
> `docker run` = `docker create` + `docker start`  
>  
> Split them for more control when needed.  
>  
> Always clean up regularly with:
> ```bash
> docker system prune
> ```

---

## 🧰 **Creating a Dockerfile**

A `Dockerfile` defines how your image is built. Example:

```Dockerfile
# Use a base image
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy files
COPY ./package.json ./
RUN npm install
COPY ./ ./

# Expose port and start
EXPOSE 3000
CMD ["npm", "start"]
```

Build and run:
```bash
docker build -t username/project:tag .
docker run -p 3000:3000 username/project:tag
```

---

## ⚡ **Performance & Build Cache**

Docker caches layers between builds.  
If a line in your Dockerfile changes, **only that line and the ones after it** are rebuilt.

🔹 **Tip:**  
Keep frequently changing lines (like `COPY ./ ./`) **at the bottom** to maximize cache efficiency.

---

## 🏷️ **Image Tags**

```bash
docker build -t username/project:tag .
docker tag <image_id> username/project:tag
```

🧩 **Notes:**
- Tags help version images easily.  
- `alpine` images are **minimal and lightweight**, great for production.

---

## 📦 **COPY Command**

```Dockerfile
COPY ./ ./                # Copy everything to working directory
COPY localFile /targetDir # Copy specific files
```

---

## 🌐 **Mapping Ports**

```bash
docker run -p 3000:8080 <image_name>
```

➡️ **Format:** `-p <host_port>:<container_port>`  
This makes the container’s internal port accessible on your machine.

---

## 🔄 **Copying Changed Files Efficiently**

```Dockerfile
COPY ./package.json ./
RUN npm install
COPY ./ ./
```

✅ This ensures dependencies are only reinstalled when `package.json` changes.

---

## 🛑 **Stopping Containers**

```bash
docker run -d redis
docker stop <container_id>
```

Stops a running container by its ID or name.

---

## 🧩 **Docker Compose**

Compose simplifies running multi-container applications.

Example usage:
```bash
docker-compose up -d
docker-compose down
```

Restart policies:
```bash
complete please chatgpt!

on-failure
always
"no"
unless-stopped
```

Look for running compose:
```bash
docker compose ps -> looks for the docker-compose file and tries to find the sames containers that have those service names
```
Build container without default Dockerfile:
```bash
docker build -f Dockerfile.dev .
```
Attach command
```bash
chat gpt gimmi a hand!
```
Using volumes for hot reloading!
```bash
chat gpt gimmi a hand!
```
💡 **Notes:**
- Each **service name** in `docker-compose.yml` becomes the container’s hostname.  
- Example repo: [TiagoSeabraCorreia/docker-redis](https://github.com/TiagoSeabraCorreia/docker-redis)

---

### 🧭 **Quick Reference Recap**

| Action | Command |
|--------|----------|
| Run test image | `docker run hello-world` |
| Create/start container | `docker create ...` / `docker start ...` |
| Execute command inside | `docker exec -it <id> <cmd>` |
| Clean system | `docker system prune [-a]` |
| Build image | `docker build -t user/app:tag .` |
| Compose build | `docker-compose up -d --build` |
| Compose up/down | `docker-compose up -d` / `docker-compose down` |

---
