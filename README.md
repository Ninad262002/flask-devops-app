# Flask DevOps App 🚀

A simple Python Flask application containerized with Docker and deployed automatically to AWS EC2 using a CI/CD pipeline built with GitHub Actions.

---

## 📌 Project Overview

This project demonstrates a complete DevOps workflow:
- A Python Flask application with REST endpoints
- Containerized using Docker
- Automated testing using Pytest
- CI/CD pipeline using GitHub Actions
- Docker image stored on DockerHub
- Deployed automatically to AWS EC2

---

## 🏗️ Architecture

```
Developer (Windows)
       │
       │  git push
       ▼
┌─────────────────┐
│    GitHub       │
│  Repository     │
└────────┬────────┘
         │
         │ Triggers GitHub Actions Pipeline
         ▼
┌─────────────────────────────────────┐
│         GitHub Actions              │
│                                     │
│  Job 1: Run Tests (pytest)          │
│         ↓ (if tests pass)           │
│  Job 2: Build Docker Image          │
│         → Push to DockerHub         │
│         ↓ (if build passes)         │
│  Job 3: SSH into EC2                │
│         → Pull latest image         │
│         → Deploy container          │
└──────────────┬──────────────────────┘
               │
       ┌───────┴────────┐
       │                │
       ▼                ▼
┌─────────────┐  ┌─────────────┐
│  DockerHub  │  │   AWS EC2   │
│   (Image    │  │  (Running   │
│   Registry) │  │  Container) │
└─────────────┘  └─────────────┘
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| Python + Flask | Application |
| Pytest | Automated Testing |
| Docker | Containerization |
| GitHub Actions | CI/CD Pipeline |
| DockerHub | Docker Image Registry |
| AWS EC2 | Cloud Deployment |

---

## 📁 Project Structure

```
flask-devops-app/
├── app.py                        # Flask application
├── requirements.txt              # Python dependencies
├── test_app.py                   # Automated tests
├── Dockerfile                    # Docker image definition
├── .dockerignore                 # Files to exclude from Docker build
├── .gitignore                    # Files to exclude from Git
├── README.md                     # Project documentation
└── .github/
    └── workflows/
        └── ci-cd.yml             # GitHub Actions pipeline
```

---

## 🔗 Application Endpoints

| Endpoint | Method | Description | Response |
|---|---|---|---|
| `/` | GET | Home endpoint | App info and version |
| `/health` | GET | Health check | `{"status": "healthy"}` |

---

## ⚙️ CI/CD Pipeline

The pipeline is defined in `.github/workflows/ci-cd.yml` and has 3 jobs:

### Job 1 — Run Tests
- Triggered on every push to `main` branch
- Sets up Python 3.11
- Installs dependencies
- Runs `pytest test_app.py`
- Pipeline stops if any test fails

### Job 2 — Build & Push Docker Image
- Runs only if Job 1 passes
- Builds Docker image from `Dockerfile`
- Pushes image to DockerHub

### Job 3 — Deploy to EC2
- Runs only if Job 2 passes
- SSHs into AWS EC2 instance
- Pulls latest Docker image from DockerHub
- Stops old container
- Starts new container on port 80

---

## 🚀 How to Run Locally

### Without Docker

```bash
# Clone the repository
git clone https://github.com/YourUsername/flask-devops-app.git
cd flask-devops-app

# Create virtual environment
python -m venv venv
venv\Scripts\activate        # Windows
source venv/bin/activate     # Linux/Mac

# Install dependencies
pip install -r requirements.txt

# Run the app
python app.py
```

Visit: **http://localhost:5000**

---

### With Docker

```bash
# Build the image
docker build -t flask-devops-app .

# Run the container
docker run -p 5000:5000 flask-devops-app
```

Visit: **http://localhost:5000**

---

## 🧪 How to Run Tests

```bash
pip install pytest
pytest test_app.py
```

Expected output:
```
2 passed in 0.5s
```

---

## 🔐 GitHub Secrets Required

To run the CI/CD pipeline, add these secrets in your GitHub repository:

**Settings → Secrets and variables → Actions → New repository secret**

| Secret Name | Description |
|---|---|
| `DOCKER_USERNAME` | Your DockerHub username |
| `DOCKER_PASSWORD` | Your DockerHub access token |
| `EC2_HOST` | Your EC2 public IP address |
| `EC2_USER` | EC2 username (`ubuntu` or `ec2-user`) |
| `EC2_SSH_KEY` | Contents of your EC2 `.pem` private key file |


## 📝 Key Learnings

- How to containerize a Python application using Docker
- How to write a multi-job CI/CD pipeline using GitHub Actions
- How to securely store credentials using GitHub Secrets
- How to automatically deploy a Docker container to AWS EC2
- How automated testing acts as a safety net in CI/CD pipelines
- How to connect multiple DevOps tools into a complete workflow

---

## 👨‍💻 Author

**Ninad**

