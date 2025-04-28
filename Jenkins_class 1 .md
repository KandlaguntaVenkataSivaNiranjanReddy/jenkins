
#  Getting Started with Jenkins: CI/CD Made Simple

In modern software development, **automation** is the key to speed and reliability. That’s where **Jenkins** shines — helping developers integrate, test, and deploy code faster than ever.

---

## What is Jenkins?

**Jenkins** Jenkins is an open-source automation server primarily used to implement Continuous Integration (CI) and Continuous Delivery/Deployment (CD) in software development. It helps automate the building, testing, and deployment of applications, making development faster and more reliable.
- **Continuous Integration (CI)** – Automatically test and integrate code.
- **Continuous Delivery/Deployment (CD)** – Automatically deliver or deploy code to production.

> **Built With:** Java  
> **Originally Called:** Hudson  
> **Created By:** Kohsuke Kawaguchi

---

## Jenkins Workflow: Simplified Flow

1. Developer commits code to GitHub.
2. Jenkins detects the change.
3. It pulls the code and builds it (using tools like Apache Maven).
4. Jenkins runs test cases and checks code quality using **SonarQube**.
5. It creates **artifacts** (build files).
6. Artifacts are stored in repositories like **Nexus**.
7. Jenkins sends out build results (email, Slack, etc.).
8. Optionally, Jenkins can deploy the build to a server like **Tomcat** (Continuous Deployment).

![KK Funda](https://github.com/user-attachments/assets/5f79cd33-ab66-4930-8662-7b69399f066e)



---

## Why is Jenkins So Popular?

- **Free & Open Source**
- **1,800+ Plugins** – Integrates with Git, Docker, Kubernetes, etc.
- **Strong Community Support**
- 🛠**Customizable Pipelines** – Declarative or Scripted
- **Cross-Platform** – Works on Windows, Linux, macOS

---

## Advantages of Using Jenkins for CI/CD

- Faster Development Cycles
- Early Bug Detection
- Better Code Quality
- Easy Rollbacks
- Improved Team Collaboration
- Consistent Releases

---

## Jenkins Alternatives

| Tool             | Description                                      |
|------------------|--------------------------------------------------|
| **GitLab CI/CD**     | Built-in GitLab CI/CD pipelines                |
| **GitHub Actions**   | Automation directly within GitHub              |
| **CircleCI**         | Cloud-based, fast and simple                   |
| **Travis CI**        | Loved by open-source projects                  |
| **TeamCity**         | Enterprise-friendly (JetBrains)                |
| **Bamboo**           | From Atlassian, integrates with Jira           |

All of these integrate well into modern DevOps stacks.

---

## Continuous Delivery (CD)

**CD** ensures that your application is always ready to deploy. It automates everything except the final deployment.

### CD Pipeline:

```
Code → Unit Tests → Integration → Acceptance Tests → ✅ Ready for Manual Deployment
```

### Why CD?

- Code is always **in a deployable state**
- Final push to production is **manual (1-click)**
- Ideal for **client-based projects**
- Safer than full automation, with all other steps still automated

- Client-Based Projects
Use Continuous Integration (CI) + Continuous Delivery (CD)

Code is always in a deployable state

Final deployment is manual – typically just one click

🛠Ensures high quality, consistent releases, and quick feedback

Safer than Continuous Deployment, while still benefiting from automation

---

## Continuous Deployment

**Continuous Deployment** takes CD to the next level — even the final deployment is automated.

### Deployment Flow:

```
Code → Unit Tests → Integration → Acceptance Tests → 🚀 Auto Deployment
```

### Why Continuous Deployment?

- Latest code goes **live instantly**
- No human involvement in deployment
- Perfect for **fast-paced internal tools** or **mature pipelines**
---
# Jenkins Installation Guide

## LTS (Long Term Support) Version

The Jenkins LTS version is the stable version without any issues. To install Jenkins on RedHat/Fedora/Centos, follow the steps below.

## Step 1: Switch to Root User
```bash
sudo su -
```

## Step 2: Install Required Packages
```bash
yum install wget tree -y
```

## Step 3: Add Jenkins Repository and Install Jenkins
```bash
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
sudo yum upgrade
sudo yum install fontconfig java-17-openjdk
sudo yum install jenkins
```

## Step 4: Reload Systemd and Enable Jenkins Service
```bash
sudo systemctl daemon-reload
sudo systemctl enable jenkins
```

## Step 5: Start Jenkins
```bash
systemctl status jenkins
systemctl start jenkins
systemctl status jenkins
```

## Step 6: Access Jenkins Web Interface
Make sure to enable port `8080` on your firewall.

Open the Jenkins web interface in your browser:
```
http://<your-server-ip>:8080/
```

Retrieve the Jenkins initial admin password:
```bash
cat /var/lib/jenkins/secrets/initialAdminPassword
```

## Step 7: Set Up Jenkins
- Click on "Install suggested plugins."
- Create an admin user:
    - Username: `kkfunda`
    - Password: `kkfunda`
    - Confirm Password: `kkfunda`
    - Full Name: `KK FUNDA`

- Save and continue.

Once done, click "Save and Finish" to complete the setup and start using Jenkins.

References:

Official Jenkins Documentation - Installing Jenkins

Jenkins on RedHat/CentOS Guide


