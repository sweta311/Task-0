Task-0: CI/CD Pipeline with Jenkins

This repository contains a `Jenkinsfile` that defines an automated Continuous Integration and Continuous Deployment (CI/CD) pipeline. The pipeline is designed to check out code, perform static code analysis, and deploy a web application to a target server.

## Pipeline Overview

This declarative Jenkins pipeline automates the following key processes:

1.  **Checkout**: Pulls the latest code from a specified branch of a Git repository.
2.  **Code Quality Analysis**: Integrates with a SonarQube server to perform static code analysis, ensuring code quality and identifying potential bugs or vulnerabilities.
3.  **Deployment**: Securely deploys the application files to a remote server using SSH and restarts the web server (Nginx) to apply the changes.

### Pipeline Stages

The pipeline is broken down into the following stages:

| Stage                | Description                                                                                                                              |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| **Checkout**         | Clones the `uat` branch from the specified Git repository using pre-configured GitHub credentials.                                       |
| **SonarQube Analysis** | Executes the SonarQube scanner to analyze the source code. The results are sent to the SonarQube server for review.                     |
| **Deploy HTML**      | Uses `sshagent` to securely connect to the deployment server, copies all repository files, and then restarts the Nginx service.          |

The pipeline also includes a `post` block to provide feedback on whether the deployment succeeded or failed.

---

## Prerequisites and Configuration

To successfully run this pipeline, the Jenkins environment must be configured with the following:

#### 1. Jenkins Plugins:
*   Pipeline
*   Git Plugin
*   SSH Agent Plugin (`sshagent`)
*   SonarQube Scanner for Jenkins Plugin

#### 2. Jenkins Tools:
*   A SonarQube Scanner installation configured with the name `sonarqube`.

#### 3. Jenkins Credentials:
*   `Github`: A credential (e.g., username/password or Personal Access Token) with access to the source code repository.
*   `ssh-snapcut`: An SSH private key credential for securely accessing the deployment server.

#### 4. Jenkins Global Variables & Secrets:
This pipeline relies on variables and secrets managed by Jenkins to avoid hardcoding sensitive information.

*   **Secrets (`credentials` block):**
    *   `DEPLOY_SERVER`: The IP address or hostname of the deployment server.
    *   `DEPLOY_USER`: The SSH user for the deployment server.
    *   `PROJECT_KEY`: The project key for SonarQube.
    *   `HOST_URL`: The URL of the SonarQube server.
    *   `LOGIN_TOKEN`: An authentication token for the SonarQube server.
*   **Variables (`vars` block):**
    *   `DEPLOY_PATH`: The absolute path on the deployment server where files should be copied (e.g., `/var/www/html`).
    *   `GITHUB_URL`: The full Git URL of the source code repository to be checked out.

---

## How to Use

1.  Ensure all prerequisites listed above are configured in your Jenkins instance.
2.  Create a new **Pipeline** job in Jenkins.
3.  In the "Pipeline" section of the job configuration, select **"Pipeline script from SCM"**.
4.  Choose **"Git"** as the SCM.
5.  Enter the URL of this repository (`https://github.com/your-username/Task-0.git`).
6.  Ensure the "Branch Specifier" is set to `*/main` or the branch where this `Jenkinsfile` resides.
7.  Save the job and click **"Build Now"** to run the pipeline.
