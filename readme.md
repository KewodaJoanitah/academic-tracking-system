# Academic Issue Tracking System (AITS)

## 📌 Project Overview
The **Academic Issue Tracking System (AITS)** is a web application designed to streamline the process of logging, tracking, and resolving academic record-related issues at **Makerere University**. It provides an efficient workflow for students, lecturers, and administrators to manage academic concerns transparently.

## 🚀 Features
- 📝 **Issue Logging**: Students can report academic issues related to courses, grades, or missing records.
- 📊 **Issue Tracking**: Users can monitor the status of their submitted issues in real time.
- 👩‍🏫 **Role-Based Access Control (RBAC)**: Different user roles (Student, Lecturer, Registrar) with specific permissions.
- 🔄 **API Integration**: RESTful APIs built using **Django REST Framework (DRF)**.
- 🌍 **Modern UI**: Built with **React** and styled using **Tailwind CSS**.
- 📂 **File Attachments**: Users can upload supporting documents.
- 🔐 **Authentication**: Secure user authentication and session management.
- ☁️ **Cloud Deployment**: Containerized with Docker and deployed across a multi-node infrastructure.

## 🏗️ Tech Stack
| Component       | Technology  |
|---------------|------------|
| Backend       | Django REST Framework, Gunicorn |
| Frontend      | React, Tailwind CSS |
| Database      | PostgreSQL (production), SQLite (local dev) |
| Authentication | Django Authentication, JWT |
| Containerization | Docker, Docker Compose |
| Reverse Proxy / Load Balancing | Nginx |
| Infrastructure | CloudLab (multi-node Linux servers) |

## 🐳 Deployment Architecture

AITS is containerized and deployed across a 4-node infrastructure, with each node responsible for a single part of the stack:

```
                    ┌─────────────────────┐
                    │   Nginx (node0)     │
                    │   Load Balancer     │
                    └──────────┬──────────┘
                               │
                ┌──────────────┴──────────────┐
                │                             │
     ┌──────────▼──────────┐       ┌──────────▼──────────┐
     │   React (node1)     │       │  Django + Gunicorn   │
     │     Frontend        │       │     (node2)          │
     └──────────────────────┘      └──────────┬───────────┘
                                               │
                                    ┌──────────▼───────────┐
                                    │  PostgreSQL (node3)  │
                                    │      Database         │
                                    └────────────────────────┘
```

- **node0 — Nginx**: Receives all incoming traffic and routes `/` to the React frontend and `/api/` to the Django backend.
- **node1 — React**: Serves the student, lecturer, and registrar dashboards, built and containerized with `node:18-slim`.
- **node2 — Django**: Runs the REST API behind Gunicorn (not Django's dev server), containerized with `python:3.10-slim`.
- **node3 — PostgreSQL**: Dedicated database server, replacing the original local SQLite setup.

Each service has its own `Dockerfile`, and `docker-compose.yml` at the project root ties frontend, backend, and database together for local multi-container runs.

## 🛠️ Installation & Setup

### Option 1 — Local Development (Quickest)

#### 1️⃣ Clone the Repository
```sh
git clone https://github.com/KewodaJoanitah/academic-tracking-system.git
cd academic-tracking-system
```

#### 2️⃣ Backend Setup (Django)
```sh
cd backend
python -m venv venv
source venv/bin/activate  # (Windows: venv\Scripts\activate)
pip install -r requirements.txt
python manage.py migrate
python manage.py runserver
```

#### 3️⃣ Frontend Setup (React)
```sh
cd aits-frontend
npm install
npm start
```

### Option 2 — Docker (Recommended)

Build and run each service in its own container:

```sh
# Backend
docker build -t aits-backend ./backend
docker run -d --name backend -p 8000:8000 aits-backend:latest

# Frontend
docker build -t aits-frontend ./aits-frontend
docker run -d --name frontend -p 3000:3000 aits-frontend:latest

# Database
docker run -d --name database \
  -e POSTGRES_DB=aits_db \
  -e POSTGRES_USER=aits_user \
  -e POSTGRES_PASSWORD=aits_password \
  -p 5432:5432 \
  postgres:15
```

Or bring up all three services at once with Docker Compose:

```sh
docker compose up -d
```

### Option 3 — Multi-Node Deployment (Production-style)

For deployment across separate servers (as on CloudLab):

1. Provision 4 Linux nodes and assign one role to each: load balancer, frontend, backend, database.
2. Install Docker on the frontend, backend, and database nodes.
3. Clone this repo onto the frontend and backend nodes.
4. Build and run each container on its respective node (see Docker commands above), pointing the database `HOST` in `settings.py` to the database node's address.
5. Install Nginx on the load balancer node and configure it to proxy `/` to the frontend node and `/api/` to the backend node.
6. Run `python manage.py migrate` inside the backend container to set up the database schema.

## 🎯 Usage
- **Students**: Report academic issues and track progress.
- **Lecturers**: Review and verify student issues.
- **Registrar**: Manage and resolve reported academic issues.

## 🔄 Pull Requests

### How to Contribute
We welcome contributions! Follow these steps to submit a pull request:

1. **Fork the Repository**: Click the fork button on GitHub.
2. **Clone Your Fork**:
   ```sh
   git clone https://github.com/KewodaJoanitah/academic-tracking-system.git
   cd academic-tracking-system
   ```
3. **Create a New Branch**:
   ```sh
   git checkout -b feature-branch
   ```
4. **Make Your Changes**: Implement your feature or fix.
5. **Commit Your Changes**:
   ```sh
   git add .
   git commit -m "Describe your changes"
   ```
6. **Push to GitHub**:
   ```sh
   git push origin feature-branch
   ```
7. **Create a Pull Request**: Open a PR on GitHub and describe your changes.

## 📜 License
This project is licensed under the **MIT License**.

## 🤝 Contributors
- **Nansereko Housnah**
- **Sumayah Kaswa Nabukeera**
- **Suuna Raymond**
- **Tamale Denis Valelian**
- **Kewoda Joanitah**
- **Kukunda Stacy**
- Open for collaboration! Feel free to fork and contribute. 🚀

---
🔥 *AITS – Making academic issue tracking seamless, from a single laptop to a multi-node deployment!*
