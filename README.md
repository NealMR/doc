# Experiment 8: Building a CI/CD Pipeline using Docker and Jenkins

## Aim
To build an automated Continuous Integration pipeline using Jenkins that builds a Docker image and runs a containerized test.

---

## Prerequisites
- **Docker Desktop**: Installed and running on your workstation.
- **Jenkins**: Running on `http://localhost:8080`.
- **Git & GitHub**: Git installed locally and a GitHub account.

---

## Step-by-Step Instructions

### Step 1: Open Terminal in this Folder
```bash
cd jenkins-docker-pipeline
```

### Step 2: Test Docker Build Locally (Optional)
Verify that the application and Dockerfile work on your machine before automating:
```bash
# 1. Build the Docker image
docker build -t jenkins-flask-app .

# 2. Run the container
docker run -d -p 5000:5000 --name test-app jenkins-flask-app

# 3. Test the running container
curl http://localhost:5000

# 4. Clean up test container
docker stop test-app
docker rm test-app
```

### Step 3: Push Project to GitHub
Jenkins pulls your code from a Git repository:
```bash
# 1. Initialize local repository
git init -b main
git add .
git commit -m "Initial commit for Jenkins Docker pipeline"

# 2. Link to your GitHub repository and push
git remote add origin https://github.com/YOUR_USERNAME/jenkins-docker-pipeline.git
git push -u origin main
```

### Step 4: Create a Jenkins Pipeline Job
1. Open Jenkins in your browser: `http://localhost:8080`.
2. Click **New Item** on the left dashboard.
3. Enter job name: `Docker-CI-Pipeline` -> Select **Pipeline** -> Click **OK**.
4. In the job configuration page, scroll down to the **Pipeline** section:
   - **Definition**: Select **Pipeline script from SCM**.
   - **SCM**: Select **Git**.
   - **Repository URL**: Paste your GitHub repository URL (`https://github.com/YOUR_USERNAME/jenkins-docker-pipeline.git`).
   - **Branch Specifier**: `*/main`.
   - **Script Path**: `Jenkinsfile`.
5. Click **Save**.

*(Alternative without GitHub: Under Definition, select "Pipeline script" and paste the contents of `Jenkinsfile` directly).*

### Step 5: Run the Build
1. Click **Build Now** on the left sidebar.
2. Watch the pipeline execute through the stages:
   - **Build Docker Image**: Builds the `jenkins-flask-app` image.
   - **Run Container**: Runs the containerized test application.
   - **Post Actions**: Automatically cleans up `test-app` container.

### Step 6: Verify Console Output
1. Click on the latest build number (e.g., **#1**) under Build History.
2. Click **Console Output** in the left menu.
3. Verify that the Docker image was built, the container started, and the build ends with:
```text
Finished: SUCCESS
```
