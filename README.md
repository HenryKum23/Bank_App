In this section, I'll guide you through the process of deploying the 2048 game application, which was built using React and TypeScript. The deployment process leverages a DevOps pipeline to ensure streamlined automation, secure code scanning, and easy access to the application via a browser interface.

### Tools and Technologies Used:i
1. **AWS EC2**: A cloud server to host and run the application and its services.
2. **Jenkins**: An automation server installed on the EC2 instance to manage Continuous Integration and Continuous Deployment (CI/CD) pipelines.
3. **Docker and Docker-Compose**: Used for containerizing the application and its services, facilitating efficient deployment and scaling. Docker Compose orchestrates the deployment of multiple containers.
4. **SonarQube**: A Dockerized service for static code analysis, ensuring code quality and identifying potential issues early.
5. **Trivy**: A vulnerability scanner for Docker containers that helps identify and mitigate security risks.

### Step-by-Step Process:

#### 1. **Setting Up the EC2 Instance**
The first step involved launching an AWS EC2 T2 Medium instance. After the instance was up and running, I performed a system update to ensure that all packages and dependencies were current.

#### 2. **Installing Jenkins and CI/CD Tools**
Next, I installed Jenkins on the EC2 instance to manage the CI/CD pipeline. Along with Jenkins, I set up other essential tools such as SonarQube Scanner, OWASP Dependency-Check, Eclipse Temurin (for Java support), Docker, and Docker-Compose. These tools together form the backbone of the pipeline, automating tasks like code quality checks, security scans, and the build/deployment process.

#### 3. **Running SonarQube in Docker**
Instead of setting up a separate server for SonarQube, I opted to run it as a container within Docker on the same EC2 instance (using a T2 large instance for performance). This approach minimized the need for additional infrastructure. I configured Jenkins to connect with SonarQube by generating a token within SonarQube, adding it to Jenkins credentials, and setting up a webhook in SonarQube to trigger Jenkins jobs based on the results of the quality gate checks.

#### 4. **Scanning for Vulnerabilities with Trivy**
To enhance security, I installed **Trivy**, a vulnerability scanner, on the EC2 instance. Trivy scans Docker images for known security issues and vulnerabilities, allowing me to address potential risks before deploying the application to production.

#### 5. **Building the Application Pipeline**
With the necessary tools in place, I configured the Jenkins pipeline to automate the build process for the React and TypeScript application. The pipeline is triggered whenever code changes are detected. It builds a Docker image of the application, runs it as a container, and exposes it on a specified port, making it accessible via a web browser.

#### 6. **Testing the Application in the Browser**
Once the pipeline successfully builds the Docker image, I can verify the application's functionality by accessing it through the browser on the specified port. This provides a quick and convenient way to validate the application before proceeding with the deployment.

### Summary:
This deployment pipeline integrates several tools to automate and secure the entire process. From code quality checks with **SonarQube** to security scans with **Trivy**, and the build and deployment process managed by **Jenkins** and **Docker**, the pipeline ensures high-quality, secure code at each step. Using Docker containers allows the application to run consistently across different environments, while **Kubernetes** provides scalability and flexibility for production-ready deployments.

This setup offers streamlined and repeatable process not just for deploying the 2048 game, but for any containerized application, ensuring robust security, high code quality, and seamless access during both development and production phases.

(./screenshots/integration-coverage.png)

