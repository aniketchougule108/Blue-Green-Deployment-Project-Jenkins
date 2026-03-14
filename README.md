# Production-Ready Blue-Green Deployment using Jenkins and AWS Application Load Balancer

## Project Overview
This project demonstrates **Zero-Downtime Deployment** using the Blue-Green Deployment Strategy on AWS.

The system maintains two identical environments:
* **Blue Environment** → Current production version
* **Green Environment** → New version of the application

Traffic is routed using AWS Application Load Balancer (ALB).

During deployment:

1) Jenkins deploys the new version to the inactive environment
2) Performs a health check
3) If successful → switches traffic
4) If failed → automatic rollback

This ensures no downtime during releases

---

## Architecture Diagram
Users send requests to Application Load Balancer, which forwards traffic to either the Blue or Green environment.

Deployment is controlled by **Jenkins CI/CD Pipeline.**

```
              End Users
                  │
                  ▼
        Application Load Balancer
           (Listener : Port 80)
             │             │
             ▼             ▼
       Blue Target      Green Target
          Group             Group
           │                  │
           ▼                  ▼
      Blue Environment    Green Environment
      EC2 + App v1        EC2 + App v2

                 ▲
                 │
            Jenkins Pipeline
       (Deploy + Health Check + Switch)
```
###  Technologies Used
* AWS EC2
* AWS Application Load Balancer
* Jenkins
* GitHub
* Linux
* Nginx Web Server
* AWS CLI

---
## Infrastructure Setup
### Step 1: Launch EC2 Instances
Create two EC2 instances

| Environment | Instance Name |
| ----------- | ------------- |
| Blue        | blue-server   |
| Green       | green-server  |

Security Group should allow:
```
HTTP : 80
SSH : 22
```

![](./img/Screenshot%20(359).png)


### Step 2: Install Web Server
Run on both EC2 instances:
```
sudo apt update
sudo apt install nginx -y
```
Start service:
``` 
sudo systemctl start nginx
sudo systemctl enable nginx
```
---

### Deploy Application Versions
* Version 1 (Blue Environment)  
* Version 2 (Green Environment)

---
## Load Balancer Configuration
### Step 1: Create Target Groups
Create two target groups.
| Target Group       | Instance  |
| ------------------ | --------- |
| blue-target-group  | Blue EC2  |
| green-target-group | Green EC2 |

Configuration:
```
Protocol : HTTP
Port : 80
Health Check Path : /
```
![](./img/Screenshot%20(362).png)

---

### Step 2: Create Application Load Balancer

![](./img/Screenshot%20(361).png)

---

## Jenkins Setup
Install Jenkins on a server.

Install Java:
```
sudo apt update
sudo apt install openjdk-17-jdk -y
```
Install Jenkins:
```
sudo apt install jenkins -y
```
Start Jenkins:
```
sudo systemctl start jenkins
```
Access Jenkins:
```
http://SERVER-IP:8080

```

![](./img/Screenshot%20(365).png)

---

### Install AWS CLI on Jenkins Server
```
sudo apt install awscli -y
```
Configure AWS credentials:
```
aws configure
```
Provide:
```
AWS Access Key
AWS Secret Key
Region
```
---
### GitHub Repository Structure

Example project structure:
```
blue-green-deployment
│
├── index.html
├── Readme.md
└── Jenkinsfile
```

![](./img/Screenshot%20(364).png)

### Jenkins Pipeline Flow

Pipeline performs:
1. Pull code from GitHub
2. Identify inactive environment
3. Deploy new version
4. Perform health check
5. Switch ALB traffic
6. Rollback if deployment fails


![](./img/Screenshot%20(360).png)

---
### Jenkinsfile

![](./img/Screenshot%20(363).png)

---

### Rollback Strategy

If deployment fails during health check:
1. Jenkins marks pipeline as FAILED
2. Traffic switches back to Blue Environment
3. Production remains stable

Rollback command:
```
ALB → switch back to Blue Target Group
```
---
### Deployment Workflow
```
Developer Push Code → GitHub
          │
          ▼
     Jenkins Pipeline
          │
          ▼
Deploy to Inactive Environment
          │
          ▼
      Health Check
          │
    ┌─────┴─────┐
    │           │
 Success      Failure
    │           │
    ▼           ▼
Switch Traffic  Rollback
```
---
### Expected Output
* Zero-downtime deployments
* Safe production updates
* Automatic rollback on failure
* Fully automated CI/CD pipeline

![](./img/Screenshot%20(357).png)



![](./img/Screenshot%20(358).png)


---
### Deliverables
This project includes:
* Jenkins Pipeline
* AWS ALB configuration
* Blue-Green infrastructure
* Deployment demo
* Architecture diagram
* Target group switching screenshot
---

### Conclusion

Using Blue-Green Deployment with Jenkins and AWS ALB, we achieved:

* Zero Downtime Deployment
* Automated CI/CD
* Safe Production Releases
* Quick Rollback Mechanism

This deployment strategy is widely used in production SaaS environments.