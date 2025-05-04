# Jenkins Scripted Pipeline
Jenkins Scripted Pipeline is a powerful feature of Jenkins that allows you to define and automate your **CI/CD (Continuous Integration and Continuous Delivery)** workflows using the **Groovy scripting language**.
Scripted pipelines offer:

- **Full access to programming constructs** (e.g., `if`, `for`, variables), allowing complex logic.
- **Dynamic and conditional pipelines**, making workflows adaptable.
- High flexibility and customization, suitable for complex pipeline logic.

All pipeline logic is written inside a special file called `Jenkinsfile`, which can be stored in version control systems like Git.

---

## Setup of a Jenkins Scripted Pipeline

### Step 1: Create a Jenkins Pipeline Job
- Go to **Jenkins Dashboard** → Click on **New Item**.
- Enter a name like `scriptedPL-ci-cd-job`.
- Choose **Pipeline** → Click **OK`.

#### If "Stage View" is not visible:
- Go to **Manage Jenkins** → **Manage Plugins**.
- Search for `"Pipeline: Stage View Plugin"` and install it.

---

### Step 2: Scripted Pipeline Syntax
In Jenkins Scripted Pipeline, node is a built-in function that defines where the pipeline should run. By default, if no label is specified, it runs on the built-in (master) node, which is the main Jenkins server. It allocates an executor and executes all stages inside the block.

```groovy
node {
    stage('Example') {
        // commands go here
    }
}
```
#### Explanation:
- **`node { ... }`**:
  - This block allocates an executor on a Jenkins agent (a machine where the job runs). Everything inside runs on that agent.
- **`stage('Stage Name') { ... }`**:
  - Defines a stage in the pipeline. Stages help organize and visualize the pipeline in the Jenkins UI.

---

### Step 3: Checkout Code from GitHub
#### Use Snippet Generator:
- Go to **Pipeline Job** → Configure.
- Click **Pipeline Syntax**.
- In Snippet Generator, select **git** from the dropdown.
- Fill in:
  - **Repo URL**, **Branch**, **Credentials**.
- Click **Generate**, you will get:

```groovy
git branch: 'development', credentialsId: '8d341570-e046-4d2c-8060-33c3d5d493f7', url: 'https://github.com/gangavaramdevops/maven-web-app-project-kk-funda.git'
```

#### Pipeline Script:

```groovy
node {
    stage('git checkout') {
      git branch: 'development', credentialsId: '8d341570-e046-4d2c-8060-33c3d5d493f7', url: 'https://github.com/gangavaramdevops/maven-web-app-project-kk-funda.git'
    }
}
```
Click **Build Now** to run the job. After the build, you should see success in the build history.

---

### Step 4: Configure Maven in Jenkins
- Go to **Manage Jenkins** → **Global Tool Configuration**.
- Scroll to **Maven** → Click **Add Maven**.
- Name it like **Maven-3.9.9**.
- Jenkins will install it automatically.

---

### Step 5: Build Stage
#### Defining Maven Path
```groovy
def mavenHome = tool name: 'maven3.9.9'
```
- **tool**: Finds the path of Maven configured in Jenkins.
- **def**: Declares a Groovy variable.
- **mavenHome**: Stores the full path.

#### Example:
```groovy
node {
    def mavenHome = tool name: 'maven3.9.9'

    stage('Build') {
        sh "${mavenHome}/bin/mvn clean package"
    }
}
```
After running the pipeline, the `.war` file will be created inside the `target/` directory.

---

### Step 6: SonarQube Analysis
- Make sure `pom.xml` is updated with SonarQube configuration.

#### Example:
```groovy
stage('SonarQube Report') {
   sh "${mavenHome}/bin/mvn sonar:sonar"
}
```

---

### Step 7: Upload to Nexus Repository
#### Update `settings.xml` with Nexus credentials:
Find the file:
```sh
find / -name settings.xml
```
Manually navigate to the directory:
```sh
cd /var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven\ 3.9.9/conf/
```
Edit `settings.xml`:
```sh
sudo vi settings.xml
```
Add Nexus credentials inside `<servers>`:

```xml
<server>
    <id>nexus</id>
    <username>admin</username>
    <password>kkfunda</password>
</server>
```
Configure pom.xml with Distribution Management ensure Maven deploys correctly to Nexus, update your pom.xml file with the repository details.

#### Pipeline:
```groovy
stage('Upload to Nexus') {
    sh "${mavenHome}/bin/mvn deploy"
}
```

---

### Step 8: Deploy to Tomcat Using `curl`
- Ensure Tomcat has a user with **manager-script** role in `tomcat-users.xml`.

#### Pipeline:
```groovy
stage('Deploy to Tomcat') {
    echo "Deploying WAR file using curl..."

    sh """
        curl -u kkfunda:kkfunda \
        --upload-file /var/lib/jenkins/workspace/jio-scripted-way-pipeline-develoment/target/maven-web-application.war \
        "http://3.108.194.157:8080/manager/text/deploy?path=/maven-web-application&update=true"
    """
}
```
#### **Important: Ensure Correct Details**
1. **Job Name in the path is correct** (e.g., `jio-scripted-way-pipeline-develoment`).
2. **WAR File Name is correct** (e.g., `maven-web-application.war`).

---

### 📄 `Jenkinsfile`

```groovy
node {
    def mavenHome = tool name: "maven3.9.9"

    stage('git checkout') {
        git branch: 'development',
            credentialsId: '8d341570-e046-4d2c-8060-33c3d5d493f7',
            url: 'https://github.com/gangavaramdevops/maven-web-app-project-kk-funda.git'
    }

    stage('Build') {
        sh "${mavenHome}/bin/mvn clean package"
    }

    stage('SQ Report') {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }

    stage('Deploy into Nexus') {
        sh "${mavenHome}/bin/mvn clean deploy"
    }

    stage('Deploy to Tomcat') {
        echo "Deploying WAR file using curl..."
        sh """
            curl -u kkfunda:kkfunda \
            --upload-file /var/lib/jenkins/workspace/jio-scripted-way-pipeline-develoment/target/maven-web-application.war \
            "http://3.108.194.157:8080/manager/text/deploy?path=/maven-web-application&update=true"
        """
    }
}
```

---

### Final Steps:
1. **Click "Build Now"** in Jenkins.
2. Go to **Stage View** → Verify all stages (Checkout, Build, SonarQube, Nexus Deploy, Tomcat Deploy) are green.
3. Open **Tomcat URL in Browser** → Check deployed applications.

---

# Jenkins Slack Notification

Jenkins Slack Notification is a way to automatically send messages to your Slack channel to notify your team about the status of Jenkins builds — like STARTED, SUCCESS, or FAILURE.

## try-catch-finally Block (Error Handling)

```groovy
} catch (e) {
    currentBuild.result = "FAILED"
    throw e
} finally {
    notifyBuild(currentBuild.result)
}
```

- If something goes wrong, it catches the error.
- It marks the build as FAILED.
- Then, it throws the error again, so Jenkins knows there was a problem.
- Whether it's a success or failure, it always calls the `notifyBuild()` function to send a message (like sending a WhatsApp after any event).

---

## notifyBuild Function (Sending Notification)

```groovy
def notifyBuild(String buildStatus = 'STARTED') {
```

This function sends a Slack message about the build:

- If no status is given, it uses 'STARTED' as default.

### Set Default Build Status
```groovy
buildStatus =  buildStatus ?: 'SUCCESSFUL'
```
If `buildStatus` is empty, assume everything went fine — so set it as 'SUCCESSFUL'.

---

### Message Text and Color Setup

```groovy
def colorName = 'RED'
def colorCode = '#FF0000'
def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
def summary = "${subject} (${env.BUILD_URL})"
```
- Sets the default color to red (for failure).
- **subject**: Line that shows status and job info.
- **summary**: Adds job URL so we can click and check.

---

### Change Color Based on Status

```groovy
if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
} else if (buildStatus == 'SUCCESSFUL') {
    color = 'GREEN'
    colorCode = '#00FF00'
} else {
    color = 'RED'
    colorCode = '#FF0000'
}
```

- If build **just started** → use Yellow.
- If build **passed** → use Green.
- If build **failed** → use Red.

Colors help us quickly understand what happened.

---

### Send Message to Slack
```groovy
slackSend(color: colorCode, message: summary)
```
- This line **actually sends** the message to Slack with the color and summary we prepared earlier.
- If you don’t mention the channel, the message goes to the **default Slack channel**.
- You can mention a specific channel like `channel: '#your-channel-name'` inside the `slackSend()` line.

---

## Full Scripted Pipeline Example

In Jenkins pipelines, the `env` object is a built-in variable that provides access to environment variables within the pipeline execution. It allows you to reference important details about the running job.

## Explanation of `env` Variables Used:

- **`${env.JOB_NAME}`** → Retrieves the name of the Jenkins job that is currently running.
- **`${env.BUILD_NUMBER}`** → Returns the unique build number assigned by Jenkins for this particular job execution.
- **`${env.NODE_NAME}`** → Indicates the name of the node (agent) where the pipeline is running. If it’s executing on the master, it will return `"master"`, otherwise, it will show the name of the specific agent.

```groovy
node {
   echo "git branch name: ${env.JOB_NAME}"
   echo "build number is: ${env.BUILD_NUMBER}"
   echo "node name is: ${env.NODE_NAME}"

    def mavenHome = tool name: "maven3.9.9"

    try {
        stage('git checkout') {
          git branch: 'development', credentialsId: '8d341570-e046-4d2c-8060-33c3d5d493f7', url: 'https://github.com/gangavaramdevops/maven-web-app-project-kk-funda.git'
        }
        stage('compile') {
          sh "${mavenHome}/bin/mvn compile"
        }
        stage('Build') {
          sh "${mavenHome}/bin/mvn clean package"
        }
        stage('SQ Report') {
          sh "${mavenHome}/bin/mvn sonar:sonar"
        }
        stage('Deploy into Nexus') {
          sh "${mavenHome}/bin/mvn clean deploy"
        }
        stage('Deploy to Tomcat') {
            echo "Deploying WAR file using curl..."
            sh """
                curl -u kkfunda:kkfunda \
                --upload-file /var/lib/jenkins/workspace/jio-scripted-way-pipeline-develoment/target/maven-web-application.war \
                "http://3.108.194.157:8080/manager/text/deploy?path=/maven-web-application&update=true"
            """
        }
    } catch (e) {
        // If there was an exception thrown, the build failed
        currentBuild.result = "FAILED"
        throw e
    } finally {
        // Success or failure, always send notifications
        notifyBuild(currentBuild.result)
    }
}

def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend(color: colorCode, message: summary)
}
```

---

### Slack Notification Overview

✅ **STARTED** → The build has begun.

🟢 **SUCCESS** → The build was successful and completed without issues.

🔴 **FAILURE** → Something went wrong, and the build has failed.

This integration ensures that teams are always informed about the build process, making it easy to track progress and take necessary actions quickly.

