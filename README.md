# MEAN Stack CRUD Application

A **MEAN stack (MongoDB, Express, Angular, Node.js) CRUD application** with Dockerized backend and frontend, and Nginx serving the frontend.

---

## Project Structure

<img width="632" height="586" alt="Screenshot 2026-02-24 224012" src="https://github.com/user-attachments/assets/b4c541cd-13b5-4449-9a0e-966dce85a3e4" />

---

---

## Prerequisites

- Node.js (v20+)
- npm (v10+)
- Angular CLI (for local builds)
- Docker & Docker Compose
- MongoDB (or Dockerized MongoDB)

---

## Local Setup (Without Docker)

### Backend

```bash
cd backend
npm install
npm start
# Backend runs on http://localhost:3000
```
---
### Frontend
```bash
cd frontend
npm install --legacy-peer-deps
ng build --configuration=production
# Built files are in dist/angular-15-crud
```
---

# Docker setup
## Backend Dockerfile (backend/Dockerfile)
```dockerfile
FROM node:20
WORKDIR /app
COPY package*.json ./
RUN npm install --production
COPY . .
EXPOSE 3000
CMD ["node", "server.js"]
```
## Frontend Dockerfile (frontend/Dockerfile)
```dockerfile
FROM node:20 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install --legacy-peer-deps
COPY . .
RUN npm run build -- --configuration=production

FROM nginx:alpine
COPY --from=build /app/dist/angular-15-crud /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```
---
# Build and run containers
```bash
# Backend
cd backend
docker build -t mean-app-backend .
docker run -d --name backend -p 3000:3000 mean-app-backend
```
```bash
# Frontend
cd frontend
docker build -t mean-app-frontend .
docker run -d --name frontend -p 80:80 mean-app-frontend
```
---
# Docker Compose
```yaml
# Backend
cd backend
docker build -t mean-app-backend .
docker run -d --name backend -p 3000:3000 mean-app-backend

# Frontend
cd frontend
docker build -t mean-app-frontend .
docker run -d --name frontend -p 80:80 mean-app-frontend
```
## run all services
```bash
docker-compose up -d --build
```
---
# Cloud Deployment Notes
## 1. Upload project to VM;\:
```bash
scp -i <key.pem> -r mean-app ubuntu@<vm-ip>:~
```
## 2. SSH into the VM:
```bash
ssh -i <key.pem> ubuntu@<vm-ip>
```
---
# Troubleshooting
## Disk full error
```bash
docker system prune -a --volumes -f
```
## Angular build errors in Docker: Build Angular locally first, then copy dist/ to Nginx container.

## Dependency conflicts
```bash
npm install --legacy-peer-deps
```
## TypeScript version issues: Make sure your package.json dependencies match Angular requirements (>=5.9 <6.0 for Angular 21+).
---
# Structural Daigram

<img width="1536" height="1024" alt="MEAN-APP" src="https://github.com/user-attachments/assets/edd180d5-243e-497b-bf60-22a0d3f03ca4" />

---

# Result

<img width="1894" height="958" alt="Screenshot 2026-02-25 114743" src="https://github.com/user-attachments/assets/f10720a7-f349-42de-8e68-8c7988ee50a9" />
<img width="1897" height="964" alt="Screenshot 2026-02-25 114804" src="https://github.com/user-attachments/assets/ecb08908-eeed-4c80-ae6e-0a5c669bbb9d" />

---
# License
## This project is open-source and free to use.
```
This is fully **Markdown-ready** for GitHub and will render cleanly with code blocks, headings, and sections.  

If you want, I can also make a **super short, super clean GitHub-ready README** that’s <50 lines and perfect for “clone and run” style.  

Do you want me to do that?
```
---

