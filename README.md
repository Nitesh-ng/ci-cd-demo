# 🚀 CI/CD Pipeline with Jenkins, Docker, and GitHub Webhook

## 📘 Overview
This project demonstrates a **fully automated CI/CD pipeline** using **Jenkins**, **Docker**, and **GitHub Webhooks**.  
Whenever you push code to the GitHub repository, Jenkins automatically:
1. Clones the repository  
2. Builds a Docker image  
3. Pushes the image to Docker Hub  
4. Deploys the containerized Flask app automatically 🎯  

---

## 🧰 Tools & Technologies Used
- 🐳 **Docker** – Containerization  
- ☸️ **Jenkins** – Automation Server  
- 🌐 **GitHub** – Source Control and Webhooks  
- 🔗 **Ngrok** – Public URL tunneling for local Jenkins  
- 🐍 **Flask** – Sample Python Web Application  

---

## ⚙️ CI/CD Pipeline Stages
| Stage | Description |
|-------|--------------|
| 🌀 **Clone Repository** | Jenkins pulls the latest code from GitHub |
| 🏗️ **Build Docker Image** | Builds Docker image using the `Dockerfile` |
| 📤 **Push to Docker Hub** | Pushes image to Docker Hub repository |
| 🚀 **Deploy Application** | Runs the container on a defined port (8081) |

---

## 🪜 Step-by-Step Setup

### 1️⃣ Run Jenkins in Docker
```bash
docker run -d   -u root   --name jenkins   -p 8080:8080 -p 50000:50000   -v jenkins_home:/var/jenkins_home   -v /var/run/docker.sock:/var/run/docker.sock   jenkins/jenkins:lts
```

### 2️⃣ Access Jenkins
Visit: **http://localhost:8080**  
Use the initial admin password from:
```bash
docker exec -it jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

---

### 3️⃣ Install Required Plugins
> **Manage Jenkins → Plugins → Install:**
- GitHub Integration  
- Docker Pipeline  
- Pipeline  

---

### 4️⃣ Add Docker Inside Jenkins Container
```bash
docker exec -it jenkins bash
apt update
apt install docker.io -y
```

---

### 5️⃣ Create Docker Hub Credentials
> Jenkins Dashboard → Manage Credentials → Add “dockerhub-login”  

---

### 6️⃣ Create a Pipeline Job
- Select **Pipeline** type job  
- In “Pipeline script from SCM”, select your GitHub repo  
- Jenkins will use the `Jenkinsfile` inside your repo automatically  

---

### 7️⃣ Connect GitHub Webhook
1. Go to your GitHub repo → **Settings → Webhooks → Add webhook**  
2. Payload URL: `http://<NGROK_URL>/github-webhook/`  
3. Content type: `application/json`  
4. Select: “Just the push event”  
5. ✅ Save  

---

### 8️⃣ Start Ngrok Tunnel
```bash
ngrok http 8080
```
Copy the **Forwarding URL** and use it in the GitHub Webhook payload.

---

### 9️⃣ Test the Pipeline 🚦
Make a small commit (like editing `README.md`)  
→ Jenkins should automatically trigger and redeploy the container.

Access the app at:  
👉 **http://localhost:8081**

---

## 🧠 Project Verification Steps
- ✅ Jenkins build runs automatically  
- ✅ Docker image is created and pushed to Docker Hub  
- ✅ Container redeploys successfully on every GitHub commit  

---

## 🖼️ Project Screenshots

### 1️⃣ Ngrok Tunnel Running
> Shows the Ngrok session forwarding the Jenkins webhook.
> <img width="1920" height="663" alt="Screenshot (212)" src="https://github.com/user-attachments/assets/10496b21-627b-48ed-acf1-daad1b490cdb" />



### 2️⃣ GitHub Webhook Configuration
> Displays the GitHub repository webhook setup using the Ngrok URL.
> <img width="1920" height="1080" alt="Screenshot (209)" src="https://github.com/user-attachments/assets/953a9f59-9c19-4ab8-b38c-6cdd6de8b6f2" />



### 3️⃣ Docker Personal Access Token
> Demonstrates Docker Hub token creation for Jenkins authentication.
> <img width="1920" height="1080" alt="Screenshot (210)" src="https://github.com/user-attachments/assets/b19485ab-aa0e-406d-a9f9-b3f5f32ea3de" />


### 4️⃣ Jenkins Pipeline Successful Build
> Screenshot of Jenkins showing all stages (Clone → Build → Push → Deploy) completed successfully.
> <img width="1920" height="1080" alt="Screenshot (208)" src="https://github.com/user-attachments/assets/9f0c9625-ea8e-40dc-81f3-cd62cd73f3d9" />


### 5️⃣ Application Deployment
> The application running locally confirming successful deployment via Docker.
> <img width="1920" height="1080" alt="Screenshot (211)" src="https://github.com/user-attachments/assets/be28a940-a87a-4531-a9b3-afe41b25e1f8" />

---

## 🧩 Conclusion
With this setup, the **CI/CD pipeline** automates:
- Code fetching from GitHub  
- Docker image building and pushing  
- Container deployment  

🎯 Result: **Zero manual deployment — fully automated CI/CD workflow!**

---
