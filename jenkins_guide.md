
# Jenkins Management and Best Practices

## 1. Handling Jenkins Slowness: The Public IP Problem

Jenkins is a web application that heavily depends on its configured IP address or DNS name.  
Whenever Jenkins is hosted on a server (like AWS EC2, Azure VM, etc.), a server restart may change the public IP.  
If this happens, Jenkins can behave strangely:

- The web dashboard loads slow or may break.
- Jobs that call webhooks (like GitHub) might fail.
- Email notifications may not work.

### Solution:

- SSH into your Jenkins server.
- Navigate to the Jenkins configuration path:

```bash
cd /var/lib/jenkins/
```

- Edit the IP address in:

```bash
vi jenkins.model.JenkinsLocationConfiguration.xml
```

- Search for your old IP and replace it with the new public IP.
- Restart Jenkins safely.

### Best Practice:
Assign a static IP or Elastic IP to your server to avoid manual IP changes after every reboot.

## 2. Prevent New Builds During Maintenance

### Temporarily Disable a Jenkins Job

If you need to pause builds during server maintenance or updates:

- Navigate to the specific Jenkins job you want to disable.
- Click on “Disable Project” (usually near the top of the job page).

When a job is disabled:
- Even if new commits happen (for example, pushed to GitHub), webhooks will still fire, but Jenkins will ignore them.
- No new builds will be triggered manually or automatically.

### Important

- You must have the correct permissions (Job Configure or Admin rights) to disable or enable a project.
- Existing running builds will not be affected — only new builds are blocked.
- Missed commits during the disabled period will not automatically build when the job is re-enabled.

## 3. Maven Integration Plugin in Jenkins

The Maven Integration Plugin is specially designed for Java-based projects and helps Jenkins automatically build, test, and package Java applications using Maven, directly integrating with the project’s `pom.xml`.

### How to Install

- Go to Dashboard → Manage Jenkins → Manage Plugins.
- Click on the Available tab.
- Search for Maven Integration Plugin.
- Click Install without restart (or restart later if needed).

### Key Features

- No need to manually configure Maven versions for each job.
- Jenkins will automatically detect the Maven version from the `pom.xml` file.
- Supports both local Maven installations and tool-managed Maven inside Jenkins.

## 4. Safe Restart vs Normal Restart in Jenkins

### What is Normal Restart?

- Normal Restart restarts Jenkins immediately, but it interrupts any ongoing builds.

**Impact:** If a build is in progress, it will be stopped, potentially resulting in incomplete build artifacts or failed builds.

**Use Case:** Best for situations where you need a quick restart, and you can accept that ongoing builds may be interrupted or fail.

**How to Trigger Normal Restart:**

```
http://your-jenkins-url/restart
```

**No Plugin Needed:** Normal Restart can be triggered directly via the URL.

### What is Safe Restart?

- Safe Restart waits for all ongoing builds to complete before restarting Jenkins.

**Impact:** Ensures that Jenkins does not interrupt running builds, allowing them to finish and preventing incomplete artifacts or data loss.

**Use Case:** Ideal for production environments or critical jobs, where uninterrupted builds are important.

**How to Trigger Safe Restart:**

**Plugin:** Safe Restart can also be triggered using the Safe Restart Plugin.

You can trigger a Safe Restart using the URL:

```
http://your-jenkins-url/safeRestart
```

**Note:** You can trigger a Safe Restart using either the URL or the Plugin.

## 5. Changing Jenkins Build Number

The build number in Jenkins is unique, and you can change it based on your requirements, like after a failure or to match versioning.

### When to Change the Build Number:

Changing the build number is useful in scenarios like:

- Skipping a build number after a failed or incomplete build.
- Resetting the build number for a fresh job (e.g., after a new major release or renaming the job).

### 5.1 Manually Change Build Number (Not Recommended but Possible):

Navigate to the Jenkins Job Directory:

```bash
cd /var/lib/jenkins/jobs/<job-name>/nextBuildNumber
```

Edit the Build Number:

Open the `nextBuildNumber` file and change the build number to the one you want (for example, 100).

### 5.2 Using Plugin (Recommended):

- Install the Next Build Number Plugin.

**Set the Next Build Number:**

- Navigate to the specific Jenkins Job.
- Click on "Set Next Build Number" from the job’s configuration page.
- Enter the desired build number and save.

**Scenario:**  
After completing a major release (e.g., version 2.0.0), you need the build number to match your versioning scheme (e.g., build number 2000).  
The Next Build Number Plugin makes it easy to update the build number without manually editing files.

## 6. Jenkins Audit Trail Plugin

The Jenkins Audit Trail Plugin stores log information about key actions in Jenkins, such as who created, deleted, or started a job. It also tracks changes made to job configurations, providing an audit trail of all critical actions in Jenkins.

### How to See Live Logs:

Specify a custom path to store logs.

Use the following command to view live logs:

```bash
tail -f <audit-log-file>
```

### Features:

- **Track Changes:** Keeps a record of all changes made within Jenkins.
- **Audit Actions:** Shows who performed specific actions like creating, modifying, or deleting jobs.
- **Restore Configurations:** Allows you to restore previous job configurations if needed.

### Use Cases:

- **Security Auditing:** Keep track of who made changes to critical jobs for accountability.
- **Troubleshooting:** If something breaks after a change, you can check the logs to see who made the modification and when.
- **Compliance:** Ensure you meet company or regulatory standards by tracking who did what in Jenkins.
- **Restoring Configurations:** If a job's configuration was accidentally changed, you can restore it to its previous state using the logs.

## 7. Jenkins Job Config History Plugin

The Jenkins Job Config History Plugin helps you track, review, and restore any changes made to Jenkins job configurations over time.

### Overview:

- **Track Changes:** Maintains a full history of configuration changes made to Jenkins jobs.
- **Audit Changes:** Shows who made the changes, what changes were made, and when they happened.
- **Revert Changes:** Allows you to restore a job to a previous configuration if something breaks or gets misconfigured.

### How to View and Restore:

- Go to Jenkins Dashboard → Job Config History.
- You can view:
  - A list of all configuration changes.
  - Details about deleted jobs and when they were deleted.

Click `<<` next to a previous version to restore that configuration easily.

### How Restoration Works for Deleted Jobs

If the job is still listed in Job Config History (even after deletion):

- You can click on `<<` (the "restore" button) to easily restore the old configuration without manually recreating the job folder or copying files yourself.

**❌ But if:**

- The entire job directory (job folder) was physically removed from Jenkins (`/var/lib/jenkins/jobs/`),  
**or**  
- the plugin backup is missing or corrupted,

Then you have to manually recreate the job:

- Create a new job with the same name. Upload the old `config.xml` (from your backup if available).
- Reload Jenkins to apply the configuration.

## 8. Blue Ocean Plugin for Graphical Dashboard

The Blue Ocean Plugin provides a visual interface for managing Jenkins pipelines.

- **Visual Pipeline View:** See your pipelines in a graphical, easy-to-understand layout.
- **Easier Management:** Manage and monitor your pipelines more efficiently.

### How to Use:

- Install the Blue Ocean Plugin.
- On the Jenkins Dashboard, click Open Blue Ocean to access the graphical interface.

## 9. Build Name and Description Setter Plugin

The plugin allows you to set meaningful names for your Jenkins builds, making it easier to identify them.

### How to Use:

- Go to Job → Configure.
- In the Build Environment section, select Set Build Name.
- Save the changes with a meaningful name for your build.

## 10. Build With Parameters

The Build With Parameters feature in Jenkins allows you to pass dynamic data (like branch names, version numbers, or environment settings) to your builds.  
This makes your builds more flexible, as you can specify different inputs without modifying the job configuration each time.

### How to Set Up:

- Go to Job → Configure.
- In the General section, check **This project is parameterized**.
- Add parameters (e.g., Branch Name, Version Number).
- Use `${BranchName}` inside your build scripts to reference the parameter values.

Avoid hardcoding values like branch names directly into the scripts.

## 11. Managing Views in Jenkins

### What is a View in Jenkins?

A View in Jenkins acts like a folder or group that helps you organize your jobs into logical categories.

Example: You can create separate views for different types of jobs like Airtel Jobs, Jio Jobs, Testing Jobs, or Deployment Jobs.

### Why Use Views?

- **Organize your Jenkins dashboard:** Instead of having 100 mixed jobs cluttering the dashboard, you can create separate views to easily manage and find jobs.
- **Better Job Grouping:** Helps in organizing related jobs based on teams, environments, or types of tasks.
- **Improved Efficiency:** You can quickly access the jobs that matter most without scrolling through everything.

### Steps to Create a View:

- Go to the Jenkins Dashboard.
- Click on **New View**.
- Enter a name for your view (e.g., Airtel Jobs, Jio Jobs).
- Select the jobs you want to group in this view.
- Click **OK** to save.

**Note:** Deleting a view will not delete the jobs inside it. Only the view grouping will be removed.  
The jobs will still exist in Jenkins; they’ll just no longer be part of that specific view.
