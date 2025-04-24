
# Jenkins Freestyle Job – Complete Guide with DevOps Integrations

A **Freestyle Job** in Jenkins is the most basic and flexible type of job. It allows you to configure and automate tasks through a **Graphical User Interface (GUI)**. It’s beginner-friendly but powerful enough to support full CI/CD workflows like building, testing, and deploying software.

---

## Key Features of Freestyle Jobs
-  Supports Source Code Management (Git, SVN, etc.)
- Add build steps like shell commands, Maven targets, etc.
- Configure post-build actions (e.g., email notifications, deployments)
-  Supports build triggers (e.g., poll SCM, periodic builds)

---
##  When to Use a Freestyle Job
- For **simple projects** with fewer stages.
- When doing **manual integrations** with external tools.
- When you **don’t need complex logic**, like what Pipelines provide.
- Ideal for **learning**, **quick setups**, and **POCs** (proof of concept).

---

## 🔗 DevOps Tool Integration with Freestyle Job

Here’s how Jenkins Freestyle Jobs can integrate with various tools:

---

##  Creating Your First Jenkins Freestyle Job 
1. Go to **Jenkins Dashboard → New Item**
2. Enter a name (e.g., `jio-dev`) → Select **Freestyle Project**
3. Under **General**, add a meaningful description.

---

##  Step 1: Git Integration

###  Setup Source Code Management

- Under **Source Code Management**, select **Git**
- Install Git on Jenkins server:
  ```bash
  yum install git -y
  ```
- Add GitHub credentials (Personal Access Token is recommended)
- Set the branch to build:
  ```
  */development
  ```

---

##  Step 2: Maven Configuration

### Configure Maven in Jenkins
- Go to **Manage Jenkins → Global Tool Configuration**
- Scroll to **Maven → Add Maven**
- Name it (e.g., `Maven 3.9.8`) and select version
- Jenkins supports **multiple Maven versions** — just choose the one per job.

>  In Linux CLI, switching Maven versions manually isn't this flexible.

### Build Step
- Under **Build → Invoke top-level Maven targets**
- Add goals:
  ```
  clean package
  ```

>  This compiles your code and generates a `.war` or `.jar` artifact.

---

##  Step 3: SonarQube Integration (Code Quality)

### Setup in `pom.xml`

Add the following under `<properties>`:

```xml
<sonar.host.url>http://<sonarqube-server>:9000</sonar.host.url>
<sonar.login>your-sonarqube-token</sonar.login>
```

### 🔹 Jenkins Build Configuration

Add Maven goal:
```bash
clean package sonar:sonar
```

> After the build, your project will show up in the **SonarQube Dashboard**.

---

##  Step 4: Nexus Integration (Artifact Deployment)

### 🔹 Configure `settings.xml`

Find your Maven settings file:
```bash
find / -name settings.xml
```

Update it with Nexus credentials:

```xml
<servers>
  <server>
    <id>nexus</id>
    <username>admin</username>
    <password>kkfunda</password>
  </server>
</servers>
```

Add repository URLs under `<distributionManagement>` in `pom.xml`.

### 🔹 Jenkins Build Goals

```bash
clean package sonar:sonar deploy
```

>  Now your `.war` or `.jar` is deployed to **Nexus Repository**.

---

## Step 5: Tomcat Deployment (WAR File)

### 🔹 Setup

- Go to **Manage Jenkins → Plugins**
- Install: **Deploy to Container Plugin**

### 🔹 Configure Jenkins Job

- Add **Post-build Action → Deploy WAR/EAR to a container**
- Provide:
  - **Tomcat URL**
  - **Manager credentials**
  - WAR file path (e.g., `**/maven-web-application.war`)

### 🔹 Tomcat User Permissions

Make sure `tomcat-users.xml` has:

```xml
<role rolename="manager-script"/>
<user username="admin" password="admin" roles="manager-script"/>
```

>  If credentials are incorrect → `401 Unauthorized`  
>  If roles are missing → `403 Forbidden`

---

## Bonus: CI/CD Pipeline Summary

```
🔁 Jenkins Freestyle Flow:

1️⃣ Checkout code from GitHub  
2️⃣ Build using Maven  
3️⃣ Analyze with SonarQube  
4️⃣ Deploy to Nexus  
5️⃣ Deploy to Tomcat  
```

---

##  Conclusion

Jenkins Freestyle Jobs are a great way to get started with CI/CD. Though Pipelines are more powerful, Freestyle is perfect for:

- Getting hands-on with Jenkins basics  
- Quick deployments  
- Testing integrations with tools like Git, Maven, SonarQube, Nexus, and Tomcat

---
