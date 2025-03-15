# Docker Volume Persistence: Bind Mounts on Linux Container 🐳

## 📌 Introduction
This project demonstrates how to use Docker bind mounts with a Linux container to persist data beyond a container’s lifecycle. By mounting a local directory into a container, data remains accessible even after the container is removed.

---

## 📂 Directory Structure
```
project_root/
│── Dockerfile
│── README.md
```

---

## 🔧 Steps & Observations

### 🏗 Step 1: Run a Container with a Bind Mount
```bash
docker run -dit --name alpine_with_bind_mount -v C:\Users\rythe\docker_data:/data alpine:latest sh
```

### ✅ What Happened?
- Docker pulled the `alpine:latest` image if not found locally.
- A container named `alpine_with_bind_mount` was created.
- The `-v` flag mounted the local directory `C:\Users\rythe\docker_data` to `/data` inside the container.
- The container started a shell (`sh`) in detached mode.

---

### 📄 Step 2: Create a File Inside the Bind Mount
```bash
docker exec -it alpine_with_bind_mount sh -c "echo 'Hello, Rahul!' > /data/testfile.txt"
```

### ✅ What Happened?
- A file `testfile.txt` was created inside `/data` and the text "Hello, Rahul!" was written into it.
- Since `/data` is a bind-mounted directory, the file was actually stored in `C:\Users\rythe\docker_data` on the host.

---

### ✅ Step 3: Verify the File
```bash
docker exec -it alpine_with_bind_mount sh -c "cat /data/testfile.txt"
```

#### 📌 Output:
```
Hello, Rahul!
```

---

### 🗑 Step 4: Remove the First Container
```bash
docker rm -f alpine_with_bind_mount
```

### ✅ What Happened?
- The container was forcefully stopped and removed.
- The file `testfile.txt` persisted on the host system due to the bind mount.

---

### 🔄 Step 5: Run a New Container with the Same Bind Mount
```bash
docker run -dit --name new_alpine -v C:\Users\rythe\docker_data:/data alpine sh
```

---

### 🔎 Step 6: Verify File Persistence
```bash
docker exec -it new_alpine sh -c "cat /data/testfile.txt"
```

#### 📌 Output:
```
Hello, Rahul!
```

---

## 🎯 Conclusion
✅ Bind mounts allow data persistence across multiple container instances.
✅ Deleting a container does not remove data stored in the bind-mounted directory.
✅ Any new container with the same mount can access previous container data.
✅ Useful for sharing files between containers and persisting data beyond the container’s lifecycle.

---

## 🚀 Next Steps
- 🛠 Experiment with named volumes (`docker volume create`) for more efficient storage management.
- 🐳 Try using bind mounts with different container images.
- 🔐 Explore how file permissions impact bind-mounted files between host and container.

---

## 🌐 Run the Project
```bash
git clone <repository-url>
cd docker-bind-mount
docker build -t bind-mount-demo .
```

---

## 🛑 Stop and Clean Up
```bash
docker stop new_alpine

docker rm new_alpine
```

