# Jenkins CI/CD Pipeline with SonarQube Analysis

This repository contains a Jenkins declarative pipeline that performs:
- Git source code checkout from a `uat` branch
- Static code analysis using SonarQube
- Deployment of HTML frontend to a remote server via SSH
- Nginx service restart post-deployment

##  Project Structure

##  Features

-  CI/CD integration using Jenkins
-  SonarQube static code analysis
-  SSH-based secure deployment
-  Environment variables & credentials secured via Jenkins Secrets
-  Nginx auto-restart for frontend service

## Prerequisites

Ensure the following are set up before using the pipeline:

### On Jenkins:
- Jenkins installed and running
- `sshagent` plugin installed
- SonarQube scanner installed (named `sonarqube`)
- Jenkins credentials:
  - `ssh-snapcut` (SSH private key)
  - `Github` (GitHub token or credentials)

### In Jenkins Secrets:
| Variable             | Description                           |
|----------------------|---------------------------------------|
| `DEPLOY_SERVER`      | IP or domain of remote deployment server |
| `DEPLOY_USER`        | Username on deployment server         |
| `DEPLOY_PATH`        | Remote path to deploy HTML            |
| `PROJECT_KEY`        | SonarQube project key                 |
| `HOST_URL`           | SonarQube server URL                  |
| `LOGIN_TOKEN`        | SonarQube access token                |
| `GITHUB_URL`         | Git repository HTTPS/SSH URL          |

##  Pipeline Stages

### 1. Checkout
Checks out code from the `uat` branch.

### 2. SonarQube Analysis
Runs static code analysis on the codebase using SonarQube CLI scanner.

### 3. HTML Deployment
- Deploys updated frontend files to the specified server.
- Restarts Nginx service after copying files.

##  Security Notes

- Secrets and credentials are not stored in code.
- All sensitive data is handled via Jenkins credentials and environment variables.

## Sample Output
<img width="1362" height="628" alt="Screenshot from 2025-07-25 16-21-59" src="https://github.com/user-attachments/assets/6b190e51-5fdf-43c8-997a-121c37be80fb" />

<img width="1348" height="611" alt="Screenshot from 2025-07-25 16-19-17" src="https://github.com/user-attachments/assets/a1c2dd05-1f49-4215-8df6-b7e5fb17e117" />


## Author

**Sweta Gupta**

---

## Tags

`jenkins` `ci/cd` `sonarqube` `nginx` `deployment` `devops-pipeline`


