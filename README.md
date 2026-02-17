LinkedIn Clone – Full Stack Docker Deployment on AWS EC2

A DevOps practice project: Deploying a LinkedIn-style full-stack application using Docker on AWS EC2.
I did not write the application code – this project focuses on containerization, cloud deployment, and infrastructure management.

---

📋 Table of Contents

· Overview
· Technologies Used
· Architecture
· Prerequisites
· Deployment on AWS EC2
  · 1. Launch EC2 Instance
  · 2. Connect to EC2
  · 3. Install Docker & Docker Compose
  · 4. Deploy the Application
· Restarting After EC2 Stop/Start
· Troubleshooting
· Project Structure
· Author

---

📌 Overview

This repository contains the deployment configuration for a LinkedIn clone application. The app itself is pre‑built (Flask backend + static frontend). My contribution is the Dockerization and AWS EC2 deployment, demonstrating:

· Containerization with Docker & Docker Compose
· Reverse proxy setup with Nginx
· Cloud infrastructure on AWS (EC2, Security Groups)
· Process management and recovery after instance stop/start

The goal was to gain hands‑on experience with real‑world DevOps practices.

---

🛠 Technologies Used

Component Technology
Backend Python, Flask, Gunicorn, SQLite
Frontend HTML, CSS, JavaScript (pre‑built)
Web Server Nginx (serves static files + reverse proxy)
Container Docker, Docker Compose
Cloud AWS EC2 (Amazon Linux 2023)
Version Control Git & GitHub

---

🏗 Architecture

```
User Browser
       │
       ▼
┌──────────────┐      (port 80)
│   Nginx      │  Frontend Container
│──────────────│  - Serves static files
│   Static     │  - Proxies /api to backend
│   Files      │
└──────────────┘
       │  /api
       ▼
┌──────────────┐      (port 5000)
│   Gunicorn   │  Backend Container
│   (Flask)    │  - Handles API requests
└──────────────┘
       │
       ▼
┌──────────────┐
│   SQLite     │  Persistent volume
│   Database   │
└──────────────┘
```

· Both containers run on the same EC2 instance.
· Docker volumes ensure database persistence across container restarts.
· All services are defined in docker-compose.yml.

---

✅ Prerequisites

· An AWS account with permissions to launch EC2.
· A key pair (.pem file) for SSH access.
· Basic knowledge of terminal/command line.
· Git installed locally (optional, for cloning).

---

🚀 Deployment on AWS EC2

1. Launch EC2 Instance

· AMI: Amazon Linux 2023 (free tier eligible)
· Instance type: t2.micro
· Security Group rules:
  · SSH (22) – your IP only
  · HTTP (80) – 0.0.0.0/0
  · Custom TCP (5000) – 0.0.0.0/0 (optional, for testing backend directly)
· Key pair: Select your existing key or create a new one (download the .pem).

2. Connect to EC2

Open a terminal (PowerShell on Windows, Terminal on Mac/Linux) and run:

```bash
cd /path/to/your-key-folder
ssh -i "your-key.pem" ec2-user@your-instance-public-ip
```

3. Install Docker & Docker Compose

Once logged into EC2, execute:

```bash
# Update system and install Docker
sudo yum update -y
sudo yum install docker -y
sudo systemctl start docker
sudo systemctl enable docker

# Add ec2-user to docker group (so you can run docker without sudo)
sudo usermod -a -G docker ec2-user

# Log out and back in for group changes to take effect
exit
```

Reconnect to EC2, then install Docker Compose:

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/v2.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version   # Should show v2.24.0
```

4. Deploy the Application

On your local machine, transfer the project folders to EC2 (or clone from GitHub):

```bash
# From your local project folder
scp -i "your-key.pem" -r linkedin-clone-backend ec2-user@your-instance-ip:~/
scp -i "your-key.pem" -r linkedin-clone-frontend ec2-user@your-instance-ip:~/
scp -i "your-key.pem" docker-compose.yml ec2-user@your-instance-ip:~/
```

Then on EC2:

```bash
cd ~
docker-compose build   # Build images
docker-compose up -d   # Start containers in background
docker-compose ps      # Verify both services are "Up"
curl http://localhost  # Test locally (should return HTML)
```

Finally, open your browser and visit:

```
http://your-instance-public-ip
```

You should see the LinkedIn Clone login page.

---

🔁 Restarting After EC2 Stop/Start

If your EC2 instance is stopped and later started again, the public IP may change, and Docker services will be down. To bring the app back:

1. Start the instance from AWS Console and note the new public IP.
2. Connect via SSH using the new IP.
3. Start Docker and the containers:
   ```bash
   sudo systemctl start docker
   cd ~/linkedin-clone   # or wherever your docker-compose.yml is
   docker-compose start
   ```
4. Verify with docker-compose ps and curl http://localhost.
5. Access the app at the new IP in your browser.

Tip: Use an Elastic IP to keep the public IP constant even after stop/start.

---

🔍 Troubleshooting

Problem Check / Solution
docker-compose: command not found Docker Compose not installed. Follow step 3 again.
Containers exit immediately View logs: docker-compose logs
Port 80 already in use Stop conflicting service: sudo systemctl stop nginx
Cannot connect to EC2 (timeout) Verify security group allows SSH (22) from your IP.
App loads but API fails Ensure backend container is running: docker-compose ps. Check backend logs: docker-compose logs backend.
Database not persisting Volumes may be misconfigured. Check docker-compose.yml volume definitions.

---

📂 Project Structure

```
linkedin-clone/
├── linkedin-clone-backend/
│   ├── app.py.txt                # Flask app (renamed to .py during build)
│   ├── models.py.txt             # Database models
│   ├── requirements.txt.txt      # Python dependencies
│   └── Dockerfile                 # Backend container definition
├── linkedin-clone-frontend/
│   ├── srcApp.css (GLOBAL).txt   # Styles
│   ├── srcApp.js.txt              # Main JS
│   ├── srccomponentsNavbar.*.txt  # Navbar component
│   ├── srcpagesFeed.js.txt         # Feed page
│   ├── srcpagesLogin.js.txt        # Login page
│   ├── index.html                  # Entry point
│   ├── nginx.conf                   # Nginx configuration
│   └── Dockerfile                   # Frontend container definition
├── docker-compose.yml               # Orchestrates both containers
└── README.md                        # This file
```

---

👨‍💻 Author

Ajay Yadav
AWS & DevOps fresher passionate about cloud infrastructure and automation.
This project was built to practice real‑world deployment skills.

· GitHub: @yourusername
· LinkedIn: Your LinkedIn Profile

---

📄 License

This project is for educational purposes. The application code is not original; the focus is on the DevOps implementation.

---

⭐ If you found this useful, please star the repository!
