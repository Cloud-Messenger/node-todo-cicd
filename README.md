# 📦 ECS + ECR Production Handbook — Deploy a Containerized App on AWS

☁️ **Build Today for a Bigger Tomorrow.**

*What if you could go from a clean Ubuntu box to a live AWS Fargate container by following one clear, verified handbook?*

Welcome to the **ECS + ECR Production Handbook (Edition 1.0)**, a hands-on guide that takes a containerized Node.js app from GitHub all the way to **Amazon ECS on AWS Fargate**, pulled from **Amazon ECR**, with logs streaming to **CloudWatch**. Every phase is explained, verified with a checkpoint, and followed by the hardening you need before real users arrive.

No container experience required. Just follow the phases, one step at a time! 🚀

**Created by:** cloud-messenger™
🌐 **Website:** [cloudmessenger.in](https://cloudmessenger.in/)
💼 **LinkedIn:** [Connect with Cloud-Messenger](https://www.linkedin.com/in/cloud-messenger)
🐙 **GitHub:** [Explore Cloud-Messenger projects](https://github.com/Cloud-Messenger)
🏷️ **#buildwithCloud-Messenger**

---

## 🎯 What Is This Project?

A step-by-step production handbook that merges hand-written whiteboard notes, command walkthroughs and Cloud-Messenger visual guides into **one source of truth** for deploying a Docker container on AWS.

- 🐧 Set up Ubuntu and pull the project code from GitHub.
- 🐳 Install Docker, build the image and prove it works locally.
- 🔑 Install AWS CLI v2 and verify your identity with `aws sts get-caller-identity`.
- 📦 Create an Amazon ECR repository, then tag and push your image.
- ⚙️ Create a Fargate cluster and the task execution IAM role.
- 🧱 Define the task, run it, open the right port and reach the app on its public IP.
- 🚨 Debug with a symptom → cause → fix troubleshooting matrix.
- 🛡️ Learn what changes between a lab and production.

---

## 🧪 The Lab at a Glance

| Item | Value |
|------|-------|
| **App** | Node.js To-Do (`node-todo-cicd`) |
| **Container port** | `8000 / TCP` |
| **Compute** | AWS Fargate (serverless) |
| **Registry** | Amazon ECR |
| **Region** | e.g. `ap-south-1` (Mumbai) |
| **Time to finish** | ≈ 45–60 minutes |
| **Host OS** | Ubuntu 22.04 / 24.04 on x86_64 |

---

## 🏗️ Architecture

```text
GitHub ──git clone──▶ EC2 (Ubuntu: Docker + AWS CLI)
                         │  build, tag, push
                         ▼
                   Amazon ECR (image registry)
                         │  pull
                         ▼
IAM role ─────────▶ Amazon ECS (Fargate task) ──▶ Browser  http://<public-ip>:8000
                         │
                         ▼
                   CloudWatch (container logs)
```

---

## 📖 Handbook Contents

| Section | Chapter | What You Will Learn |
|:-------:|---------|---------------------|
| **00** | Architecture & Prerequisites | The big picture, dependency chain and prerequisites checklist |
| **01** | Ubuntu & Code Setup | Launch and connect to EC2, update packages, clone the repo, read the Dockerfile |
| **02** | Docker: Build & Run | Install Docker, run it without `sudo`, build the image, map `-p 80:8000`, test locally |
| **03** | AWS CLI & Credentials | Install CLI v2, `aws configure`, verify identity, IAM permissions for the CLI user |
| **04** | Amazon ECR | Create the repository, log in, tag and push (public lab and private production variants) |
| **05** | ECS Cluster & IAM Roles | Fargate cluster, Container Insights, task execution role and policy table |
| **06** | Task Definition | Task size, platform, roles, container image, port 8000 and `awslogs` |
| **07** | Run, Secure & Access | Run the task, security group, public IP, CLI verification and logs |
| **08** | Troubleshooting Matrix | Common errors with likely causes and fixes |
| **09** | Production Hardening | Lab practice vs production practice, readiness checklist |
| **10** | Cleanup & Cheat Sheet | Cleanup order to stop the bill, one-page command cheat sheet and recap |
| **A** | Appendix | Original whiteboards, reconciliation of conflicting notes and six original Cloud-Messenger visual guides |

---

## 🧰 Quick Command Highlights

```bash
# Build and run locally (host port 80 -> container port 8000)
docker build -t nodeapp .
docker run -d --name nodeappweb -p 80:8000 nodeapp

# Prove your AWS identity
aws sts get-caller-identity

# Public ECR login (always us-east-1)
aws ecr-public get-login-password --region us-east-1 | \
  docker login --username AWS --password-stdin public.ecr.aws

# Tag and push
docker tag node-app:latest public.ecr.aws/<alias>/node-app:latest
docker push public.ecr.aws/<alias>/node-app:latest

# Check the task and follow its logs
aws ecs describe-tasks --cluster <cluster> --tasks <task-arn> \
  --query 'tasks[0].[lastStatus,stoppedReason]'
aws logs tail /ecs/nodeapp-task --follow
```

> 💡 **Fastest debugging signal:** `describe-tasks` → `stoppedReason` usually names the exact problem in one line.

---

## 🛡️ Lab vs Production

The lab proves it works. The handbook's hardening chapter covers what changes before real users arrive, including:

- Private ECR with scan on push and lifecycle rules
- Immutable image tags (git SHA or semver) instead of `:latest`
- IAM roles instead of long-lived access keys
- An Application Load Balancer with HTTPS in front of the tasks
- Private subnets, an ECS Service with 2+ tasks across AZs, and CI/CD with rolling deploys
- A supported LTS base image, a non-root user and log retention with alarms

> ⚠️ **Security:** never paste access keys into code, screenshots or Git. Attach an IAM role to the EC2 instance where you can.

---

## 🎓 Learning Outcomes

After completing this handbook, learners should be able to:

- Explain how code travels from GitHub to a running Fargate container.
- Build, tag and push a Docker image to Amazon ECR.
- Install and verify AWS CLI v2 and configure credentials safely.
- Create an ECS cluster, a task execution role and a task definition.
- Run a Fargate task, configure its security group and reach it on port 8000.
- Troubleshoot `CannotPullContainerError`, `exec format error`, timeouts and missing logs.
- Describe the differences between a lab setup and a production-ready deployment.
- Clean up resources in the right order to avoid surprise charges.

---

## 📂 Repository Structure

```text
ecs-ecr-production-handbook/
├── Cloud-Messenger-ECS-ECR-Production-Handbook.pdf   # Complete handbook
└── README.md                                         # Project documentation
```

> Add extra folders (for example, screenshots or the sample app) if you want to expand the repository.

---

## 🚀 How to Use This Handbook

### Option 1: Read online
Open **Cloud-Messenger-ECS-ECR-Production-Handbook.pdf** in your GitHub repository. GitHub renders PDFs in the browser.

### Option 2: Download
1. Open the PDF in the repository.
2. Click **Download raw file**.
3. Read it offline on desktop or mobile.

### Option 3: Clone the repository
```bash
git clone https://github.com/Cloud-Messenger/ecs-ecr-production-handbook.git
cd ecs-ecr-production-handbook
```

*Replace the repository URL if your project is hosted under a different repository name or account.*

### Follow the lab
The sample app used throughout the handbook lives here:

```bash
git clone https://github.com/Cloud-Messenger/node-todo-cicd.git
cd node-todo-cicd
```

---

## ✅ Prerequisites

- An AWS account with billing alerts enabled
- An IAM user (not root) with ECR and ECS permissions
- An Ubuntu EC2 instance (`t3.micro` is enough) with your key pair saved
- A security group allowing SSH (22) from your IP
- Access to the `node-todo-cicd` repository
- A default VPC with a public subnet for the Fargate task
- One chosen AWS region, used consistently

---

## 💡 Why This Handbook Matters

Most container tutorials stop at "it works on my machine." Others jump straight to production without explaining the steps in between.

This handbook connects both: a clear, verified lab you can finish in about an hour, plus an honest look at what to change before production. It also reconciles conflicting notes (public vs private ECR, ARM vs x86, task sizes and open ports) so you always know which choice the guide makes and why.

---

## 🤝 Learn, Build, and Share

Designed for students, beginners, aspiring Cloud and DevOps Engineers, and anyone shipping their first container to AWS.

- ⭐ If this handbook helps you ship, consider starring the repository.
- 🔁 Share it with someone who is just starting out.
- 💬 Suggest improvements by opening an issue.

---

## ☁️ #buildwithCloud-Messenger

**Learn • Build • Visualize • Troubleshoot • Grow**

*Made with care by cloud-messenger™*

🌐 [cloudmessenger.in](https://cloudmessenger.in/)
💼 [LinkedIn — Cloud-Messenger](https://www.linkedin.com/in/cloud-messenger)
🐙 [GitHub — Cloud-Messenger](https://github.com/Cloud-Messenger)

*Making cloud and DevOps concepts visual, practical, and accessible to everyone.*

---

## ⚠️ Copyright Notice

© 2026 cloudmessenger. The compilation, structure, layout, design and branding of this workbook are the original work of cloudmessenger. No part may be reproduced, redistributed or resold without written permission.

Cloud Messenger™ is a trademark of cloud-messenger.
