
# 🚀 Project Rudra – CI/CD Pipeline  
### GitLab → Jenkins → Maven → Nexus → Docker → Tomcat

![Pipeline Demo](screenshots/demo.gif)

A complete CI/CD pipeline built entirely using Docker containers. This README includes screenshots and a GIF demo section for rich documentation.

---

## 📘 Overview
Project Rudra automates:

- GitLab → Source Code  
- Jenkins → CI/CD  
- Maven → Build  
- Nexus → Artifact Storage  
- Docker → Image Build  
- Tomcat → Deployment  

---

## 🖼️ Screenshots  

### 🟦 GitLab Project  
![GitLab Screenshot](screenshots/gitlab.png)

### 🟧 Jenkins Pipeline  
![Jenkins Screenshot](screenshots/jenkins.png)

### 🟪 Nexus Repository  
![Nexus Screenshot](screenshots/nexus.png)

### 🟩 Deployed Application  
![Tomcat Screenshot](screenshots/tomcat.png)

---

## 🎞️ GIF Demo  
![Pipeline GIF Demo](screenshots/pipeline_demo.gif)

---

## 🧩 Architecture  
```text
GitLab → Jenkins → Nexus → Docker → Tomcat
```

---

## 🛠 Troubleshooting

| Issue | Cause | Fix |
|-------|-------|------|
| 401 Unauthorized | Wrong Nexus creds | Update credentials |
| Docker not found | Socket not mounted | Mount `/var/run/docker.sock` |
| Java errors | Wrong JDK path | Set `/opt/java/openjdk` |

---

## 📦 Files & Structure
```
project-rudra/
 ├── Jenkinsfile
 ├── Dockerfile
 ├── pom.xml
 ├── screenshots/
 │    ├── gitlab.png
 │    ├── jenkins.png
 │    ├── nexus.png
 │    ├── tomcat.png
 │    └── pipeline_demo.gif
 └── README.md
```

---

## 🎤 Interview Summary

> “Project Rudra is a complete CI/CD pipeline implemented using Docker containers, automating build, storage, image creation, and deployment seamlessly across GitLab, Jenkins, Maven, Nexus, Docker, and Tomcat.”

---

## 📥 Download Instructions  
Screenshots/GIF should be placed under the `screenshots/` folder.

