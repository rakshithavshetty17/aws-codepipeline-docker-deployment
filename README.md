# AWS DevOps Project – CI/CD Pipeline for Containerized Web Application

This project demonstrates a fully automated CI/CD pipeline built using AWS-managed DevOps tools.  
The pipeline deploys a containerized Python web application to Amazon ECS (Fargate) using AWS CodeCommit, CodeBuild, CodePipeline, and ECR.

Everything is done **fully inside AWS** — no external tools required.

---

# 📌 Project Architecture

The project uses the following AWS services:

- **CodeCommit** – Source code repository  
- **CodeBuild** – Builds Docker image, pushes to ECR  
- **ECR (Elastic Container Registry)** – Stores container images  
- **CodePipeline** – Automates entire CI/CD flow  
- **ECS Fargate** – Runs containers without managing servers  
- **IAM Roles** – Secure communication between services  

---

# 📁 Project Structure

aws-devops-cicd-project/
│── app.py
│── Dockerfile
│── buildspec.yml
│── README.md


---

# 🚀 Application Overview

### **app.py**
A simple Python Flask app returning a test message.

### **Dockerfile**
Packages the application into a Docker container.

### **buildspec.yml**
Instructions for CodeBuild to build, tag, push the image to ECR, and generate `imagedefinitions.json` for ECS deployment.

---

# 🛠️ **Steps Involved in the Project (Complete Guide)**

Below are **all the steps** followed to create this AWS DevOps project from scratch.

---

# ✅ **STEP 1 — Create CodeCommit Repository**
1. Go to **AWS Console → CodeCommit → Create repository**  
2. Name it: `webapp-repo`  
3. Open CloudShell  
4. Clone the repository:


---

# ✅ **STEP 2 — Add Project Files**
Inside the repo, create these files:

- `app.py`
- `Dockerfile`
- `buildspec.yml`

After creating files:


---

# ✅ **STEP 3 — Create an ECR Repository**
1. Go to **ECR → Create Repository**  
2. Name: `webapp-repo`  
3. Keep defaults and create.

This will store Docker images built by CodeBuild.

---

# ✅ **STEP 4 — Create ECS Cluster**
1. Go to **Amazon ECS → Create Cluster**  
2. Choose **Networking Only (Fargate)**  
3. Name: `webapp-cluster`  
4. Create cluster.

---

# ✅ **STEP 5 — Create Task Definition**
1. Go to **ECS → Task Definitions → Create new**  
2. Launch type: **FARGATE**  
3. Container name: `webapp-container`  
4. Image: (paste ECR placeholder or leave blank; CodePipeline will update)  
5. Port Mapping: `80:80`  
6. Create Task Definition.

---

# ✅ **STEP 6 — Create ECS Service**
1. ECS → Cluster → webapp-cluster  
2. Create Service  
3. Launch type: **FARGATE**  
4. Task Definition: `webapp-task`  
5. Desired tasks: `1`  
6. Networking: choose VPC + subnets + public IP enabled  
7. Create Service.

---

# ✅ **STEP 7 — Create CodeBuild Project**
1. Go to **CodeBuild → Create project**  
2. Source provider: **CodeCommit**  
3. Repo: `webapp-repo`  
4. Build environment:  
   - OS: Amazon Linux 2  
   - Runtime: Standard  
   - Privileged mode: **Enabled** (required for Docker)  

5. Buildspec: **Use buildspec.yml in repo**  
6. Create project.

---

# ✅ **STEP 8 — Create CodePipeline**
1. Go to **AWS CodePipeline → Create Pipeline**  
2. Source: **CodeCommit (webapp-repo)**  
3. Build: **CodeBuild project** you created  
4. Deploy: **ECS (Fargate)**  
5. Select:  
   - Cluster: `webapp-cluster`  
   - Service: `webapp-service`

6. Create the pipeline.

Pipeline will now run automatically.

---

# 🚀 **STEP 9 — Trigger Deployment**
Any time you push code:

The pipeline will automatically:

✔ Detect changes  
✔ Build Docker image  
✔ Push to ECR  
✔ Update ECS Task Definition  
✔ Deploy new container  





