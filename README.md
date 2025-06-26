````md
# 🧠 Final Project – MLops Dockerized App Suite

This project includes three Dockerized services:

- **`object-detection-react-app`** – React-based frontend for object detection  
- **`yolo-v5-flask-app`** – Flask-based YOLOv5 backend for real-time object detection  
- **`depth-anything-flask-app`** – Flask API for monocular depth prediction  

---

## 📦 Docker Hub Build & Push

### 🔨 Step 1: Build Images

```bash
cd object-detection-react-app && docker build . -t ashutoxh/object-detection-react-app
cd yolo-v5-flask-app && docker build . -t ashutoxh/yolo-v5-flask-app
cd depth-anything-flask-app && docker build . -t ashutoxh/depth-anything-flask-app
````

### 🚀 Step 2: Push Images to Docker Hub

```bash
docker push ashutoxh/object-detection-react-app
docker push ashutoxh/yolo-v5-flask-app
docker push ashutoxh/depth-anything-flask-app
```

> ⚠️ **Note:** In `yolo-v5-flask-app`, update `127.0.0.0` to `0.0.0.0` in the `app.py` file to make the service externally accessible inside containers.

---

## 🧪 Development Build (AWS ECR)

### 🏗️ Step 1: Build Dev Images

```bash
docker build \
  -f depth-anything-flask-app/Dockerfile \
  -t 680604704378.dkr.ecr.us-east-1.amazonaws.com/mlops/depth-anything-flask-app:dev \
  ./depth-anything-flask-app

docker build \
  -f object-detection-react-app/Dockerfile \
  -t 680604704378.dkr.ecr.us-east-1.amazonaws.com/mlops/object-detection-react-app:dev \
  ./object-detection-react-app

docker build \
  -f yolo-v5-flask-app/Dockerfile \
  -t 680604704378.dkr.ecr.us-east-1.amazonaws.com/mlops/yolo-v5-flask-app:dev \
  ./yolo-v5-flask-app
```

---

### 🔐 Step 2: Authenticate with AWS ECR

```bash
aws ecr get-login-password --region us-east-1 | \
docker login --username AWS --password-stdin 680604704378.dkr.ecr.us-east-1.amazonaws.com
```

---

### 📤 Step 3: Push Dev Images to ECR

```bash
docker push 680604704378.dkr.ecr.us-east-1.amazonaws.com/mlops/depth-anything-flask-app:dev
docker push 680604704378.dkr.ecr.us-east-1.amazonaws.com/mlops/object-detection-react-app:dev
docker push 680604704378.dkr.ecr.us-east-1.amazonaws.com/mlops/yolo-v5-flask-app:dev
```

---

## 🚀 Production Build & Push (AWS ECR)

### 🏗️ Step 1: Build Prod Images

```bash
docker build \
  -f depth-anything-flask-app/Dockerfile \
  -t 680604704378.dkr.ecr.us-east-1.amazonaws.com/mlops/depth-anything-flask-app:prod \
  ./depth-anything-flask-app

docker build \
  -f object-detection-react-app/Dockerfile \
  -t 680604704378.dkr.ecr.us-east-1.amazonaws.com/mlops/object-detection-react-app:prod \
  ./object-detection-react-app

docker build \
  -f yolo-v5-flask-app/Dockerfile \
  -t 680604704378.dkr.ecr.us-east-1.amazonaws.com/mlops/yolo-v5-flask-app:prod \
  ./yolo-v5-flask-app
```

### 📤 Step 2: Push Prod Images to ECR

```bash
docker push 680604704378.dkr.ecr.us-east-1.amazonaws.com/mlops/depth-anything-flask-app:prod
docker push 680604704378.dkr.ecr.us-east-1.amazonaws.com/mlops/object-detection-react-app:prod
docker push 680604704378.dkr.ecr.us-east-1.amazonaws.com/mlops/yolo-v5-flask-app:prod
```

---

## 📎 ECR Links (Public)

| Service                   | Image Link                                        |
| ------------------------- | ------------------------------------------------- |
| Object Detection Frontend | `docker pull ashutoxh/object-detection-react-app` |
| YOLOv5 Flask Backend      | `docker pull ashutoxh/yolo-v5-flask-app`          |
| Depth Anything API        | `docker pull ashutoxh/depth-anything-flask-app`   |


## 🛠️ Notes

* Ensure Docker Desktop is running and AWS CLI is configured.
* Environment variables like `$AWS_ACCOUNT_ID` and `$AWS_REGION` can be hardcoded or exported in your shell.
* Always test your containers locally before pushing to ECR/DockerHub.

---

## 📂 Project Structure

```
├── object-detection-react-app
│   └── Dockerfile
├── yolo-v5-flask-app
│   └── Dockerfile
├── depth-anything-flask-app
│   └── Dockerfile
└── README.md
```
