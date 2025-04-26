
# 1. Copying the Job Configuration for a Newly Created Job

Instead of recreating all build steps, triggers, and integrations (SonarQube, Nexus, etc.) manually for each new feature branch, you just copy an existing Jenkins job (like jio-dev) and change only the Git branch name.

This method saves a lot of time, reduces human errors, and keeps job configuration consistent across all branches.

## Real-Time Example:

You already have a Jenkins job for the development branch (jio-dev).

A developer starts working on a new branch called feature1.

Instead of building a new Jenkins job from scratch:

- Click **New Item**.
- Enter new job name (e.g., **jio-feature1**).
- Choose "**Copy from**" → select **jio-dev**.
- Update the Git branch to **feature1**.

Jenkins copies all its settings — build steps, SonarQube analysis, Nexus upload, Tomcat deployment tasks.

When your code has hardcoded values for URLs or IPs (e.g., SonarQube, Nexus, and Tomcat) in `pom.xml` and you're switching environments or working on a new branch, you must manually update those values.  
If you don't, the job will still point to the old server IPs, causing issues.

> **Ensure the correct environment URLs are set to avoid failures.**  
> This step is crucial for smooth deployments and builds.

---

# 2. Build Triggers in Jenkins

Build triggers in Jenkins are mechanisms that automatically start the build process based on specific conditions or events, such as code changes.  
This means you don’t have to manually click "Build Now" every time, saving time and ensuring more efficient workflows.

## 2.1 Poll SCM

- Jenkins checks the Git repository at regular intervals (defined by the cron schedule).
- If it detects a new commit or code update within that time period, Jenkins will automatically trigger a build based on the changes (new commit ID, updated code, etc.).

**Example Cron Schedule:**

```
H/5 * * * *
```

- This will check the repository every 5 minutes for any new commits.
- If new commits are found, Jenkins triggers a build.

**When to use:**

- When GitHub webhooks are not working or unavailable.
- If your project is hosted on an internal Git server without webhook support.

---

## 2.2 Build Periodically

- **Build Periodically**: Jenkins does not wait for updated code — it simply follows the scheduled time.
- Jenkins triggers a build at the specified intervals, whether there are new commits or not.
- Even if the source code hasn't changed, the build will still run based on the defined schedule.

The "Build Periodically" option in Jenkins is used to trigger builds based on regular time intervals.

**Example Cron Schedule:**

```
H 1 * * *
```

(Runs every day at 1 AM, regardless of code changes.)

**When to Use Build Periodically:**

- You want to run nightly builds at a fixed time every day.  
  (Example: Build the project every night at 1 AM.)

- You want to generate regular reports automatically.  
  (Example: Create a test coverage report every morning.)

---

## 2.3 GitHub Webhook

Whenever a developer makes a change (pushes code or updates) in the GitHub repository, GitHub immediately sends a message to Jenkins saying "start the build now."

- Add a webhook in the GitHub repository:  
  URL:
  ```
  http://<jenkins-url>/github-webhook/
  ```
- Set **Content-Type** to **application/json**.
- In Jenkins, enable **GitHub hook trigger for GITScm polling** in the job configuration.

**Why it is useful:**

- As soon as the code is pushed, Jenkins starts the build immediately.
- It reduces the burden on Jenkins servers (compared to continuous polling like "Poll SCM").
- It provides instant feedback to developers — faster build and test cycles!

---

# 3. Discard Old Builds

Jenkins can automatically delete older builds after a certain number of builds or after a specific number of days.  
This helps save space on your server by removing old build data.

## Real-Time Use Case:

If your Jenkins job accumulates a large number of builds over time (e.g., hundreds of builds), your server storage may start getting filled with unnecessary build data.  
To optimize disk space, you might want to retain only recent builds and automatically delete older ones.

In your Jenkins job, enable the **"Discard Old Builds"** option.

Then, set:

- **Max builds to keep**: Choose how many builds you want to keep (e.g., 10 builds).
- **Days to keep builds**: Set how long you want to keep builds (e.g., 7 days).

Jenkins will automatically clean up old builds that are beyond the set limit.

You’ll only have the latest builds left, so the server won’t run out of space.

---

## Where Are Builds Stored?

In the Jenkins server, build data is stored in this path:

```
/var/lib/jenkins/jobs/<job-name>/builds/
```

Example for a job called **jio-dev**:

```
/var/lib/jenkins/jobs/jio-dev/builds/
```

Each build will have its own folder here (like #1, #2, #3...), containing logs, metadata, artifacts, etc.  
When you enable "Discard Old Builds," Jenkins will automatically delete old build folders from this location.

> **Note:**  
> The "Discard Old Builds" option only removes Jenkins build records (logs, metadata).  
> It does not delete Nexus artifacts, SonarQube results, or any other external data.  
> These must be managed separately.

---

# 4. Timestamp in Console Output

The **Timestamper Plugin** allows you to show a timestamp next to each step in the console output of Jenkins builds.  
This helps in tracking the exact time each step is executed.

## Real-Time Use Case:

When you're debugging or optimizing your builds, you might want to know which step took longer to identify slow or problematic stages.

- Install the **Timestamper Plugin** from Jenkins Plugin Manager.
- Enable it in your job configuration.

**Result:**

Once configured, your console output will show a timestamp for every line.  
This is useful for analyzing slow steps and improving build performance.

---

# 5. JaCoCo - Code Coverage Tool

**JaCoCo** checks how much of your code is covered by tests (unit tests, integration tests, etc.).  
It ensures that important parts of the code are properly tested.

- In your Jenkins job, go to **Post-build Actions** → Add **"Record JaCoCo coverage report."**

You can set rules like:

> **Example:** Code coverage must be above 80% to pass the build.

---

## When to Use:

- When your project requires high code quality.
- If you want to block deployments for code with low coverage.
- If the team wants Jenkins to fail the build automatically when test case coverage is poor.

But 100% coverage is not always necessary — focus on critical parts of the code.
