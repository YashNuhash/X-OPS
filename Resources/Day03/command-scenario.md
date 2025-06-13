
---

## 🐧 Terminal Session: Dockerizing a React App with Multi-Stage Build

```bash
nuhash@DESKTOP-AP1UF99:~$ codeplayground
```

```bash
nuhash@DESKTOP-AP1UF99:/mnt/c/Users/NUHASH/CodePlayGround$ ls
todoapp-docker
```

```bash
nuhash@DESKTOP-AP1UF99:/mnt/c/Users/NUHASH/CodePlayGround$ cd todoapp-docker
```

```bash
nuhash@DESKTOP-AP1UF99:/mnt/c/Users/NUHASH/CodePlayGround/todoapp-docker$ ls
README.md  package-lock.json  package.json  public  src
```

```bash
nuhash@DESKTOP-AP1UF99:/mnt/c/Users/NUHASH/CodePlayGround/todoapp-docker$ ls -lrt
total 696
drwxrwxrwx 1 nuhash nuhash   4096 Jun 13 14:16 src
drwxrwxrwx 1 nuhash nuhash   4096 Jun 13 14:16 public
-rwxrwxrwx 1 nuhash nuhash    913 Jun 13 14:16 package.json
-rwxrwxrwx 1 nuhash nuhash 706092 Jun 13 14:16 package-lock.json
```

*(Mistyped commands skipped)*

```bash
nuhash@DESKTOP-AP1UF99:/mnt/c/Users/NUHASH/CodePlayGround/todoapp-docker$ vi Dockerfile
```

---

## 🐳 DockerFile

```dockerfile
# Stage 1: Build the frontend
FROM node:18-alpine AS installer

# Set working directory
WORKDIR /app

# Install dependencies
COPY package*.json ./
RUN npm install

# Copy rest of the app and build
COPY . .
RUN npm run build

# Stage 2: Serve with Nginx
FROM nginx:latest AS deployer

# Copy the built app from the previous stage to Nginx's public directory
COPY --from=installer /app/build /usr/share/nginx/html

```

---

## 🐳 Docker Build

```bash
nuhash@DESKTOP-AP1UF99:/mnt/c/Users/NUHASH/CodePlayGround/todoapp-docker$ docker build -t multi-stage .
```

<details>
<summary>📄 Click to expand Docker build logs</summary>

```
[+] Building 87.3s (14/14) FINISHED
 => [internal] load build definition from dockerfile ...
 => [installer 1/6] FROM node:18-alpine ...
 => [installer 2/6] WORKDIR /app
 => [installer 3/6] COPY package*.json ./
 => [installer 4/6] RUN npm install ...
 => [installer 5/6] COPY . .
 => [installer 6/6] RUN npm run build ...
 => [deployer 1/2] FROM nginx:latest ...
 => [deployer 2/2] COPY --from=installer /app/build /usr/share/nginx/html
 => exporting to image ...
 => => naming to docker.io/library/multi-stage
```

</details>

---

## 🖼️ List Docker Images

```bash
nuhash@DESKTOP-AP1UF99:/mnt/c/Users/NUHASH/CodePlayGround/todoapp-docker$ docker images
REPOSITORY    TAG       IMAGE ID       CREATED          SIZE
multi-stage   latest    5f168b153939   26 seconds ago   194MB
hello-world   latest    74cc54e27dc4   4 months ago     10.1kB
```

---

## ▶️ Run Docker Container

```bash
nuhash@DESKTOP-AP1UF99:/mnt/c/Users/NUHASH/CodePlayGround/todoapp-docker$ docker run -it -dp 3000:3000 multi-stage
3416a1d70225f0a7bb86bcccaf61ee3cd22b30e01129b9bf59168304714913d7
```

---

## 📦 List Running Containers

```bash
nuhash@DESKTOP-AP1UF99:/mnt/c/Users/NUHASH/CodePlayGround/todoapp-docker$ docker ps
CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS          PORTS                                               NAMES
3416a1d70225   multi-stage   "/docker-entrypoint.…"   10 seconds ago   Up 10 seconds   80/tcp, 0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   stupefied_babbage
```

---

## ❌ Typo: Incorrect Docker Log Command

```bash
nuhash@DESKTOP-AP1UF99:/mnt/c/Users/NUHASH/CodePlayGround/todoapp-docker$ docker log 3416a1d70225
docker: 'log' is not a docker command.
See 'docker --help'
```

---

## 📋 Correct Logs Output

```bash
nuhash@DESKTOP-AP1UF99:/mnt/c/Users/NUHASH/CodePlayGround/todoapp-docker$ docker logs 3416a1d70225
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2025/06/13 10:53:20 [notice] 1#1: using the "epoll" event method
2025/06/13 10:53:20 [notice] 1#1: nginx/1.27.5
2025/06/13 10:53:20 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14)
2025/06/13 10:53:20 [notice] 1#1: OS: Linux 6.6.87.1-microsoft-standard-WSL2
2025/06/13 10:53:20 [notice] 1#1: start worker processes
2025/06/13 10:53:20 [notice] 1#1: start worker process 29
...
2025/06/13 10:53:20 [notice] 1#1: start worker process 35
```

---

