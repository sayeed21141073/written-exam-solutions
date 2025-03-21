# Answer to the Question No. 01


### Part 01:
I use a variety of DevOps and DevSecOps tools in my daily workflow. Here’s a description of how I utilize them:

**CI/CD Tools (GitLab CI/CD, GitHub Actions):**  
I use GitLab CI/CD and Jenkins to automate the build, test, and deployment processes. This automation ensures that every code change is validated through automated tests and securely deployed to various cloud environments.

**Containerization & Orchestration (Docker, Kubernetes):**  
I use Docker to containerize applications, which ensures portability across different environments. Kubernetes manages these containerized applications by automatically handling scaling, service discovery, and resource allocation.

**Infrastructure as Code (Terraform, Ansible):**  
I use Terraform to define and provision cloud infrastructure as code, making deployments repeatable and scalable. Ansible is utilized for configuration management, automating software installations, updates, and system configurations across multiple servers.

**Security Tools (SCA, Snyk, SonarQube, OWASP ZAP):**  
I utilize Software Composition Analysis (SCA) to identify risks in dependencies. I use SonarQube and Snyk for static code analysis, while OWASP ZAP scans applications for runtime security vulnerabilities.

**Cloud Platforms (AWS):**  
I deploy and manage cloud infrastructure using AWS. I utilize services such as EC2 for computing, S3 for storage, IAM for access control, RDS for managed databases, Lambda for serverless computing, VPC for network isolation, CloudFront for content delivery, and ECS/EKS for container orchestration.

**Monitoring & Logging (Prometheus, Grafana, ELK Stack):**  
I use Prometheus and Grafana to monitor the performance of both infrastructure and applications in real-time.


### Part 02:

**A specific technical challenge I resolved using these tools:**

I tackled a technical challenge in a CI/CD pipeline for a cloud-deployed application where deployments failed. The issues stemmed from sensitive credentials being exposed in logs and security vulnerabilities in container images. My objective was to secure these sensitive credentials and implement security checks without hindering the speed of the deployment process.

**Resolved Process:**
- **Implemented HashiCorp Vault for Secret Management:**  
  Removed hardcoded secrets and configured Vault integration with GitLab CI/CD to inject credentials securely at runtime. Used AWS IAM roles instead of storing static AWS keys in environment variables.

- **Enhanced Security Scanning in CI/CD Pipeline:**  
  Integrated Trivy (SCA) to scan container images for vulnerabilities before deployment. Used SonarQube (SAST) & Snyk (SAST) to analyze code quality and dependency security. Added OWASP ZAP (DAST) to perform dynamic security testing post-deployment.

- **Optimized Pipeline Performance:**  
  Used GitLab CI/CD caching to speed up security scans. Implemented parallel execution for SAST and DAST to reduce overall pipeline time.


### Part 03:

**When a security scan passes locally but fails in CI, here is how I should troubleshoot the pipeline:**

1. **Check the logs and errors:**  
   The detailed errors are present in the CI pipeline logs. Compare it with the local output to find out why it failed. For example, if using GitLab CI/CD, check the pipeline that failed to view detailed logs for each job within the pipeline (e.g., build, test, deploy).

2. **Ensure environment consistency:**  
   Ensure that the CI/CD runner has the same OS and dependencies installed as the local setup. The file path is another crucial factor here. Ensure that the correct file paths are used in the scripts.

3. **Verify environment variables:**  
   Improperly set or missing environment variables can cause scripts to fail because they can't access the necessary values. This can cause issues like missing authentication tokens, database connections, or incorrect configuration values.

After checking all these things, we can troubleshoot the pipeline accurately and thus solve the errors.


# Answer to the Question No. 2:

Here is a step-by-step guide to setting up a CI/CD pipeline with GitLab CI/CD:


### Step 01: Create a new project in GitLab


### Step 02: Create a `.gitlab-ci.yml` in the root directory

It's a YAML file that configures the CI/CD pipelines. It contains various components, such as stages, jobs, scripts, artifacts, variables, cache, services, runners, etc. We need to define the configuration for the CI/CD pipeline.

Here is an example of a `.gitlab-ci.yml` with parallel execution and credential management:

```bash
stages:
  - build
  - test
  - deploy

variables:
  TEST_VAR: "This is just a test variable."

build_job:
  stage: build
  script:
    - echo "Build complete using secret key: $SECRET_KEY"

test_job_1:
  stage: test
  script:
    - echo "Test 1 complete!"

test_job_2:
  stage: test
  script:
    - echo "Test 2 complete!"

deploy_job:
  stage: deploy
  script:
    - echo "Deployment complete!"
```


### Step 03: Setting Up Credential Management

We can securely store sensitive information by adding masked or protected variables in the GitLab project's CI/CD settings. These variables can then be accessed within `.gitlab-ci.yml` scripts using the `$VARIABLE_NAME` syntax.


### Step 04: Run The Pipeline

Push the changes to the GitLab repo, and the pipeline will be triggered automatically.

- **Parallel Test Execution** is implemented by defining multiple jobs within the same stage. By default, these jobs are executed simultaneously.
- **Credentials Management** is implemented using the `$SECRET_KEY`.


# Answer to the Question No. 03:

Here are the common security practices and tools I follow to secure cloud infrastructure and deployments:

- **Access Control:**  
  Using AWS IAM, I follow the principle of least privilege, which means granting users the minimum level of access to perform their tasks. I also implement Multi-Factor Authentication (MFA) to add another layer of security.

- **Data Protection:**  
  I use AWS KMS to encrypt data, ensuring that data is protected with strong encryption. This also allows control over who has access to the data.

- **Network Security:**  
  I use security groups to control inbound and outbound traffic and VPCs to isolate cloud resources within a private network.


  # Answer to the Question No. 04:


  To stay updated with the latest developments in DevOps and Cloud technologies, I regularly:

- Read industry blogs
- Attend online and in-person events
- Subscribe to online courses
- Engage with fellow professionals on platforms like LinkedIn



# Hands-On:


## 1.1 Infrastructure Setup:

Here is the Github link of solution for creating a simple AWS infrastructure using Terraform:

[GitHub Repository: aws-terraform-project](https://github.com/sayeed21141073/aws-terraform-project)

### Approach:

1. **First, create AWS provider in `main.tf` file**
2. **Create a VPC** with one public subnet and another private subnet
3. **Add another private subnet** for RDS Availability Zone coverage, as the requirement is to have two private subnets to cover a minimum of 2 AZs
4. **Set up a Security Group for the EC2 instance**
5. **Launch an EC2 instance** in the public subnet
6. **Configure a Security Group for RDS**
7. **Define an RDS Subnet Group**
8. **Create the RDS Instance**

More details can be found in the `main.tf` file. 

### Handling Statefile Conflicts:

There are several ways to handle statefile conflicts if multiple engineers run `terraform apply` simultaneously:

- **Use CI/CD Pipelines:**  
  Manage the `terraform apply` command through CI/CD pipelines to ensure that the apply stage is centralized, reducing the chance of manual conflicts.

- **Store the `terraform.tfstate` File Remotely:**  
  Storing the `terraform.tfstate` file in a remote location like AWS S3 will centralize it, allowing multiple users to work collaboratively without conflicts.

## 1.2 Docker and K8S:

Here is the GitHub repository link where the Dockerfile and deployment YAML file are available:

[GitHub Repository: docker-and-k8s-node.js](https://github.com/sayeed21141073/docker-and-k8s-node.js)

Here is the outline of the deployment steps:

1. **Build Docker image:**
    ```bash
    docker build -t sayeed21141073/nodejs-app:latest .
    ```

2. **Push to registry:**
    ```bash
    docker push sayeed21141073/nodejs-app:latest
    ```

3. **Apply deployment YAML:**
    ```bash
    kubectl apply -f deployment.yaml
    ```

4. **Apply service YAML:**
    ```bash
    kubectl apply -f service.yaml
    ```

5. **Verify deployment and service:**
    ```bash
    kubectl get deployments
    kubectl get pods
    kubectl get services
    ```

### Steps to Troubleshoot and Prepare RCA for OOMKilled Error:

**OOMKilled** means "Out of Memory Killed" error, which occurs when a pod tries to use more memory than assigned. Let’s discuss the diagnostic and RCA steps.

### Diagnostic Step:

- Check the pod’s events using the following command:
```bash
  kubectl describe pod <pod-name> -n <namespace>
```
This command provides detailed information about the pod. Look for events related to memory usage.

### RCA Steps:

RCA, or Root Cause Analysis, is the process of identifying the causes of problems from the system level to resolve the issues. The steps to troubleshoot and prepare RCA for an OOMKilled error are given below:

1. **Identify the Pod Name, Time of the OOMKilled Event, and Namespace:**

   - Use the earlier mentioned command:
     ```bash
     kubectl describe pod <pod-name> -n <namespace>
     ```

2. **Check the Pod’s Memory Limits and Requests:**

   - Use the following command to view the YAML configuration:
     ```bash
     kubectl get pod <pod-name> -o yaml
     ```

3. **Recommendations:**

   - If necessary, recommend increasing the memory limit.
   - If the limit is sufficient, investigate the reasons for excessive memory usage and take necessary steps to resolve the issues.


# Real-World Scenarios:

### Scaling Web App Hosted On AWS:

Here is the list of approaches to scale and improve the performance of a web application experiencing high traffic:

1. **Analyze Metrics:**
   - Use AWS CloudWatch to monitor metrics like CPU usage, memory, and traffic to gain insights into performance and health.

2. **Scale the Application:**
   - Set up an Auto Scaling group for the EC2 instances.
   - Use Elastic Load Balancing (ELB) to distribute incoming traffic across multiple instances.

3. **Optimize Database Performance:**
   - For relational databases, consider using Amazon RDS.
   - For NoSQL databases like DynamoDB, enable auto-scaling to adjust read and write capacity.

4. **Ensure Redundancy and High Availability:**
   - Deploy the application to multiple Availability Zones (AZs).

5. **Implement Caching:**
   - Use Amazon CloudFront and ElastiCache to implement caching.

6. **Set Up Monitoring and Alerts:**
   - Use CloudWatch Alarms to get notified of any performance issues.

After implementing these steps, I can scale the infrastructure to handle traffic smoothly and ensure high availability.

### Cost Management: How to Reduce AWS Cost by 20%

To reduce cloud costs by 20% without compromising performance, these strategies can be followed:

1. **Use Cost-Analyzing Tools:**
   - Utilize AWS Cost Explorer to track and analyze spending.
   - Consider third-party cost optimization tools like Cloudability for additional insights.

2. **Leverage AWS Trusted Advisor:**
   - Get recommendations on current usage and adjust the size of resources based on actual needs.

3. **Utilize Spot Instances:**
   - For non-critical workloads, using spot instances can significantly reduce costs.

4. **Automate with Infrastructure as Code (IaC):**
   - Use tools like Terraform or Pulumi to automate resource provisioning and management. This ensures consistency and reduces manual errors, which can help lower costs.

By implementing these strategies, I can effectively reduce cloud costs by 20% while maintaining performance.


