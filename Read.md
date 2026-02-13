LinkedIn Clone - Docker Deployment on AWS EC2

Project Overview
A LinkedIn-style full stack web application built to practice DevOps deployment on AWS EC2 using Docker. This project focuses on containerization and cloud deployment, not Python coding.

Technology Stack
Backend: Flask (Python) - I didn't write this code

Frontend: HTML/CSS/JavaScript - I didn't write this code

Container: Docker & Docker Compose

Cloud: AWS EC2 (Amazon Linux 2023)

Web Server: Nginx

Project Structure
linkedin-clone/
├── linkedin-clone-backend/
│   ├── app.py.txt
│   ├── models.py.txt
│   ├── requirements.txt.txt
│   └── Dockerfile
├── linkedin-clone-frontend/
│   ├── srcApp.css (GLOBAL).txt
│   ├── srcApp.js.txt
│   ├── srccomponentsNavbar.css.txt
│   ├── srccomponentsNavbar.js.txt
│   ├── srcpagesFeed.js.txt
│   ├── srcpagesLogin.js.txt
│   ├── index.html
│   ├── nginx.conf
│   └── Dockerfile
├── docker-compose.yml
├── .gitignore


Deploy on AWS EC2 - Step by Step
Step 1: Launch EC2 Instance
Go to AWS Console → EC2 → Launch Instance

Name: linkedin-clone-app

AMI: Amazon Linux 2023 (Free tier)

Instance type: t2.micro (Free tier)

Key pair: Create new or use existing (download .pem file)

Security Group Rules:

SSH (22) → My IP

HTTP (80) → Anywhere (0.0.0.0/0)

Custom TCP (5000) → Anywhere (0.0.0.0/0)

Click Launch Instance

Step 2: Connect to 

