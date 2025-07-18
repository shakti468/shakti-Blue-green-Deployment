# 🟦🟩 Blue-Green Deployment - Node.js + MongoDB 

This project demonstrates a **Blue-Green Deployment** using Docker and Kubernetes for a Node.js application with MongoDB. It includes:

- A backend Node.js + Express server connected to MongoDB
- Two frontend versions (`frontend-blue` and `frontend-green`)
- A plan for containerization and Kubernetes blue-green deployment

---

## ✅ Step 1: Local Deployment 

### 📦 Project Structure
```bash
shakti-Blue-green-Deployment/
├── backend/
├── frontend-blue/
├── frontend-green/
```

### 🔧 1. Install Dependencies

```bash
# Navigate into each folder and run:
npm install
```
## ⚙️MongoDB Setup
### 🌍 .env file for backend:
```bash
PORT=5000
MONGO_URI=mongodb+srv://shaktiranjanm827:X7fe2LuFrLpNE8ND@cluster0.yf0dx.mongodb.net/bluegreenDB?retryWrites=true&w=majority
```
## Start Services
Start all components in separate terminals:

### Backend
```bash
cd backend
node server.js
```

### Frontend Blue
```bash
cd ../frontend-blue
node server.js
```

### Frontend Green
```bash
cd ../frontend-green
node server.js
```
## Functionality Check
- The backend server runs on: http://localhost:5000

- Frontend-blue: http://localhost:3100

- Frontend-green: http://localhost:3200

## 📸 Screenshots

### ✅ Backend server running with health check
<img width="1650" height="713" alt="Screenshot 2025-07-18 204435" src="https://github.com/user-attachments/assets/e2800935-36f9-4cce-a8d4-0d91197cac4b" />

### ✅ Frontend-blue working and user registration
<img width="1597" height="976" alt="image" src="https://github.com/user-attachments/assets/dac2769f-dc30-4168-9af1-06584f4aa782" />

### ✅ Frontend-green working 
<img width="1627" height="945" alt="image" src="https://github.com/user-attachments/assets/f617dcd3-6f93-44e8-9bf8-b7e56138cccf" />

### ✅ MongoDB with stored user data
<img width="1228" height="675" alt="image" src="https://github.com/user-attachments/assets/14c5140d-d6d8-4910-8d2a-382c936eeee8" />


# 🐳 Step 2: Containerization - Blue-Green Deployment Skill Test

This section documents the containerization of a full-stack Node.js + MongoDB application with **blue-green frontend architecture** using **Docker** and `docker-compose`.

---

## 📁 Project Structure
```bash
├── backend/
│ ├── Dockerfile
│ └── ...
├── frontend-blue/
│ ├── Dockerfile
│ └── ...
├── frontend-green/
│ ├── Dockerfile
│ └── ...
├── docker-compose.yml
```

## ⚙️ Dockerfiles

Each component (backend, frontend-blue, frontend-green) includes a Dockerfile.

### ✅ Backend Dockerfile (port: 5000)

```dockerfile
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 5000
CMD ["node", "server.js"]
```

### Frontend-Blue Dockerfile (port: 3100)
```bash
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3100
CMD ["node", "server.js"]
```

### Frontend-Green Dockerfile (port: 3200)
```bash
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3200
CMD ["node", "server.js"]
```

📦 docker-compose.yml
```bash
version: "3.8"

services:
  mongodb:
    image: mongo
    container_name: mongodb
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db

  backend:
    build: ./backend
    container_name: backend
    ports:
      - "5000:5000"
    depends_on:
      - mongodb
    environment:
      - MONGO_URL=mongodb://mongodb:27017/bluegreenDB

  frontend-blue:
    build: ./frontend-blue
    container_name: frontend-blue
    ports:
      - "3100:3100"
    depends_on:
      - backend

  frontend-green:
    build: ./frontend-green
    container_name: frontend-green
    ports:
      - "3200:3200"
    depends_on:
      - backend

volumes:
  mongo-data:
```

## Build All Services
```bash
docker-compose build
```
### Screenshots
<img width="1009" height="457" alt="Screenshot 2025-07-19 030515" src="https://github.com/user-attachments/assets/af74d14a-2681-489b-848a-51490429ab39" />


## Run All Containers

```bash
docker-compose up
```
### Screenshots
<img width="1357" height="478" alt="Screenshot 2025-07-19 030552" src="https://github.com/user-attachments/assets/f8c9c692-3401-40a6-9fcd-b591f78794bd" />


## Stop All Containers
```bash
docker-compose down
```
