A fully containerized DevOps CI/CD pipeline designed for automation, deployment, and interview-ready demonstrations.
Based entirely on Docker containers, this pipeline simulates a real production workflow end-to-end.

🗂️ Table of Contents

📘 Overview

🛠 System Requirements

🐳 Container Setup

⚙ Jenkins Configuration

📁 Project Files

🚦 Pipeline Stages

🔍 Verification

🧩 Architecture

🛠 Troubleshooting

🎤 Interview Summary

📘 Overview

A production-like pipeline where:

📝 GitLab stores source code

🔧 Jenkins builds + orchestrates CI/CD

🏗 Maven packages the WAR

🗄 Nexus stores artifacts

🐳 Docker builds the app image

🐱 Tomcat hosts the final deployed app

All components run in Docker containers for easy setup, teardown & reproducibility.

🛠 System Requirements
Component	Specification
OS	RHEL / CentOS / Ubuntu
CPU	4 cores
RAM	8 GB
Disk	30 GB
Tools	Docker, curl, vim
🐳 Container Setup
<details> <summary>🔵 <strong>GitLab CE Setup</strong></summary>
docker run -d \
  --hostname gitlab.local \
  --name gitlab \
  --network cicd-network \
  -p 8081:80 -p 443:443 -p 2222:22 \
  gitlab/gitlab-ce:latest


Access GitLab:
👉 http://localhost:8081

</details>
<details> <summary>🟣 <strong>Nexus Repository Setup</strong></summary>
docker run -d \
  --name nexus \
  --network cicd-network \
  -p 8082:8081 \
  sonatype/nexus3


Retrieve password:

docker exec -it nexus cat /nexus-data/admin.password


Create:

maven-releases

maven-snapshots

User: jenkins

</details>
<details> <summary>🟠 <strong>Jenkins Server Setup</strong></summary>
docker run -d \
  --name jenkins \
  --hostname jenkins.local \
  --network cicd-network \
  -p 8080:8080 -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /usr/bin/docker:/usr/bin/docker \
  jenkins/jenkins:lts-jdk17


Access Jenkins:
👉 http://localhost:8080

</details>
⚙ Jenkins Configuration
<details> <summary>🛠 Global Tools</summary>
Tool	Config
JDK	Java17 → /opt/java/openjdk
Maven	Auto-install
</details>
<details> <summary>🔑 Credentials</summary>
ID	Type	Usage
gitlab-creds-token	Secret Text	GitLab token
nexus-creds	Username/Password	Nexus deploy
</details>
<details> <summary>🧩 Maven settings.xml</summary>

Path: /var/jenkins_home/.m2/settings.xml

<settings>
  <servers>
    <server>
      <id>nexus-releases</id>
      <username>${env.NEXUS_USER}</username>
      <password>${env.NEXUS_PASS}</password>
    </server>
    <server>
      <id>nexus-snapshots</id>
      <username>${env.NEXUS_USER}</username>
      <password>${env.NEXUS_PASS}</password>
    </server>
  </servers>
</settings>

</details>
📁 Project Files
<details> <summary>📦 <strong>pom.xml</strong></summary>

Defines Nexus repositories + packaging config.

</details>
<details> <summary>🐳 <strong>Dockerfile</strong></summary>
FROM tomcat:10-jdk17
COPY target/project-rudra.war /usr/local/tomcat/webapps/
EXPOSE 8080
CMD ["catalina.sh", "run"]

</details>
<details> <summary>🤖 <strong>Jenkinsfile</strong> (full CI/CD stages)</summary>

Stages:
✔ Checkout
✔ Build WAR
✔ Upload to Nexus
✔ Docker build
✔ Tomcat deploy

</details>
🚦 Pipeline Stages
Stage	Description
🔄 Checkout	Clone from GitLab
🏗 Build	mvn clean package
📤 Upload	Deploy artifact to Nexus
🐳 Image Build	Build Docker image
🚀 Deploy	Run container on port 8085
🔍 Verification
docker ps --format "table {{.Names}}\t{{.Ports}}"


App URL:
👉 http://localhost:8085/project-rudra/

🧩 Architecture
GitLab → Jenkins → Nexus → Docker → Tomcat

🛠 Troubleshooting
Issue	Cause	Solution
401 Nexus	Wrong creds	Fix user perms
Release redeploy blocked	Using release version	Switch to SNAPSHOT
Docker not found	Missing socket mount	Mount /var/run/docker.sock
Connection refused	Network issue	Reconnect to cicd-network
JAVA_HOME invalid	Wrong path	Use /opt/java/openjdk
🎤 Interview Summary (Ready to Speak)

“Project Rudra is a complete CI/CD pipeline built entirely with Docker containers. It automates GitLab commits through Jenkins builds, Maven packaging, Nexus artifact storage, Docker image creation, and deployment on Tomcat. I resolved issues like Nexus authentication, Java toolchains, and Docker socket access, resulting in a production-like automated pipeline.”
