<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=24&pause=1000&color=00FF00&center=true&width=800&lines=Swift+DevOps+Integration%3A;Automating+Deployment+for+Rapid+Delivery" />
</p>

<p align="center">
  <img src="https://img.shields.io/badge/DevOps-Automation-blue?style=for-the-badge&logo=dev.to" />
  <img src="https://img.shields.io/badge/Ansible-Automated-red?style=for-the-badge&logo=ansible" />
  <img src="https://img.shields.io/badge/Jenkins-CI/CD-orange?style=for-the-badge&logo=jenkins" />
  <img src="https://img.shields.io/badge/Docker-Containerization-blue?style=for-the-badge&logo=docker" />
  <img src="https://img.shields.io/badge/Monitoring-Grafana-yellow?style=for-the-badge&logo=grafana" />
</p>

# Swift DevOps Integration: Automating Deployment for Rapid Delivery
This project aims to implement a DevOps lifecycle swiftly across our organization. It involves provisioning essential software through Ansible, establishing an efficient Git workflow, integrating Jenkins for automated testing and deployment, Dockerizing the application for containerization, and setting up a Jenkins pipeline for seamless CI/CD processes. Additionally, it includes the integration of Prometheus and Grafana for comprehensive monitoring and analytics capabilities.

## Table of Contents

- [Project Requirements](#Project_Requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Architecture](#architecture)

## Project Requirements
**Infrastructure Setup:**
1. Operating System: Ubuntu 22.04 LTS 
2. Instances:
    - Master Node
    - 2 Worker Nodes (Slaves)

**Networking Requirements:**
1. Communication: Ensure all nodes can communicate with each other over the network.
2. Ports: Open necessary ports(jenkins:8080, SSH, HTTP, HTTPS) as per security best practices.

**Hardware Requirements**
1. Minimum Configuration(t2.micro):
   - CPU: 1v
   - Memory: 1 GiB
2. Recommended Configuration(t2.medium)
   - CPU: 2v
   - Memory: 4 GiB


## Installation

1. Installing Ansible on Ubuntu:[Ansible Commands](https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html#installing-ansible-on-ubuntu)
2. Installing Jenkins on Ubuntu:[Jenkins Commands](https://www.jenkins.io/doc/book/installing/linux/#debianubuntu)
3. Installing Java on Ubuntu: `sudo apt install openjdk-17-jre`
4. Installing Docker on Ubuntu: `sudo apt install docker.io`

## Usage
> [!NOTE]
> **1. Efficient Infrastructure Provisioning**: Using Ansible, you can automate the provisioning of essential software and configurations across your infrastructure. This ensures consistency and reduces the time required for manual setup, leading to faster deployment of environments.

> [!NOTE]
> **2. Streamlined Git Workflow:** By establishing an efficient Git workflow, teams can collaborate seamlessly on code changes, track revisions, and ensure version control. This enhances code quality, facilitates code reviews, and reduces the risk of conflicts in development.

> [!NOTE]
> **3. Automated Testing and Deployment:** Integrating Jenkins enables automated testing and deployment pipelines. This automation accelerates the release process, improves reliability by detecting issues early in the development cycle, and ensures that deployments are consistent and repeatable.

> [!NOTE]
> **4. Containerization with Docker:** Dockerizing applications allow for efficient deployment and scaling across different environments, from development to production. It provides isolation, simplifies dependency management, and promotes a microservices architecture, enhancing flexibility and resource utilization.

> [!NOTE]
>**5. CI/CD Pipelines with Jenkins:** Setting up Jenkins pipelines for CI/CD streamlines the delivery of software updates. Developers can automatically trigger builds, run tests, and deploy changes to production or staging environments based on predefined workflows. This results in faster time-to-market for new features and bug fixes.

> [!NOTE]
> **6. Monitoring and Analytics with Prometheus and Grafana:** Integrating Prometheus for monitoring and Grafana for visualization provides real-time insights into system performance, application metrics, and infrastructure health. This proactive monitoring helps identify issues promptly, optimize resource usage, and ensure high availability of services.

> [!NOTE]
> **7. Overall Efficiency and Scalability:** By implementing these DevOps practices, organizations improve their software delivery processes' overall efficiency, scalability, and reliability. Teams can focus more on innovation and less on manual tasks, leading to higher productivity and better alignment with business goals

### Steps
**1. Launch EC2 Instances:**
   - Master Instance:
     - Launch an Ubuntu 22.04 LTS instance (t2.micro).
     - Assign a security group allowing inbound traffic on SSH (port 22), HTTP, and HTTPS for administration.
   - Slave Instances (x2):
     - Launch two additional Ubuntu 22.04 LTS instances (t2.micro).
     - Assign the same security group as the master instance.
 

**2. Install Ansible on Master Instance:**
  Refer to the command given in the ***Installation part***

**3. Create Ansible Cluster**
   - Generate an SSH Key on the ***Master Machine***
   - Run the following command and hit enter three times:
     
     `ssh-keygen`
   - Retrieve the public key:

     `sudo cat /home/ubuntu/.ssh/id_rsa.pub`
   - Copy the SSH Key to the ***Test Machine***
     
     `cd .ssh`
     
     `ls`
     
     `sudo nano authorized_keys`
   - Paste the copied SSH key into the authorized_keys file, then save and exit for test and prod machine:
       - Paste the SSH key
       - Press `Ctrl+S` to save
       - Press `Ctrl+X `to exit
   -  Configure Ansible on the Master Machine:
       - Navigate to the Ansible directory:

         `cd /etc/ansible`
         
         `ls`
        - Open the host file to edit:

          `sudo nano hosts`
        - Add the Private IP addresses of both slaves:
    
          `[Test]
          <Private IP of Test>`
          
          `[Prod]
          <Private IP of Prod>`
       - Save and exit:
         - Press `Ctrl+S` to save
         - Press `Ctrl+X `to exit
- Test Ansible Configuration
 ` ansible -m ping all`

**4. Create Ansible Playbook for Dependencies**
> [!NOTE]
> Available in the Repository with the name of the ***play.yaml***


**5. Create Scripts for Master and Slaves**
> [!NOTE]
> Available in Repository with the name of the ***master.sh*** and ***slaves.sh***
  - Run the Playbook:
    
    `ansible-playbook play.yaml --syntax-check`
    
    `ansible-playbook play.yaml --check`
    
    `ansible-playbook play.yaml`
> [!IMPORTANT]
>- Check Jenkins and Java on master:
> `java --version`
>- Check java and docker on Prod and Test:
> `java --version` 
> `docker --version`

**6. Launch Jenkins Dashboard:**
   - Copy the public IP of the master go to a new tab and run it with port 8080.
     
     Public IP:8080
   - Copy the path of Jenkins where the password is given.
     
     (/var/lib/jenkins/secrets/initialAdminPassword)
   - Go back to your master:
     
     `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
   - Copy the password
   - Go back to Jenkins dashboard
     - Paste Password
     - Continue
     - Install suggested plugins
     - Fill the login details as per your choice
     - Continue and finish.


**7. Create Dockerfile on GitHub**
> [!NOTE]
> Available in the Repository with the name of the ***Dockerfile***

**8. Configure Nodes and Jobs:**

**Creation of Nodes**
  - Node Name
     - Test
     - Choose a Permanent agent
   - Remote root directory:
     `/home/ubuntu/jenkins/`
   - Launch method:
     - Choose ***via ssh***
     - Configure Host:
       - Paste the private IP of the Test
   - Create Credentials:
     - Add > Jenkins
       - Kind: `SSH username with private key`
       - Username: `ubuntu`
       - Private key: `enter directly`>  `Paste the Pem file`>  `Add`
   - Host key Verification:
     `Non-verifying strategy`
   - Save and Refresh
> [!IMPORTANT]
> Similarly for Prod Node

**Configuration of Test Job**
  - Go to the Jenkins Dashboard
    - Click New Item
      - Name: Job1
      - Choose a Freestyle project
      - Click on OK
  - Git hub project
    - Add GitHub repository where all the files are available
  - Click on Restrict where the project runs
    - Select the Test Job
  - Source Code Management:
    - Add the same GitHub repository
    - Specify the branch
      `Develop`
  - Click on Github SCM polling:
    - Copy the main URL of the Jenkins Dashboard
      - Navigate To Github
      - Go On Settings
      - Select Webhook
      - Click on Add Webhook
      - Paste the URL ***url/github-webhook/**
      - Add
> [!TIP]
> - Click the link
> - Go on Recent Deliveries to verify the connection

  - Navigate back to the dashboard
  - Save and Build
> [!IMPORTANT]
> Similarly for another test job for the Master branch and a Prod job for the master branch.


**9. Install Grafana and Prometheus**
**Installation of Grafana:**
   - Add Grafana APT Repository
     
     `sudo apt-get update`
     
     `sudo apt-get install -y software-properties-common wget`
     
     `wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -`
     
     `sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"`
   - Install Grafana

     `sudo apt-get update`
     
     `sudo apt-get install grafana`
   - Start and Enable Grafana Service:
     
     `sudo systemctl start grafana-server`
     
     `sudo systemctl enable grafana-server`
   - Access Grafana Web Interface:
     - Open a web browser and navigate to http://localhost:3000 or http://your-ubuntu-machine-ip:3000.

> [!NOTE]
> - The default username is admin.
> - The default password is admin.

**Installation of Prometheus:**
   - Download the latest version of Prometheus
     
    ` wget https://github.com/prometheus/prometheus/releases/download/v2.34.0/prometheus-2.34.0.linux-amd64.tar.gz`
   - Extract the downloaded archive
     
     `tar xvfz prometheus-2.34.0.linux-amd64.tar.gz`
     

10. Configure Grafana and Prometheus**
**Configuration of Grafana:**
    - 
**Configuration of Prometheus:**
    - Navigate to the Prometheus directory:
      
     ` cd prometheus-2.34.0.linux-amd64`
    - Configure Prometheus:
> [!IMPORTANT]
> A basic example is available at the Repository
    - Start Prometheus:
      `./prometheus --config.file=prometheus.yml`
    - Access Prometheus UI: Open a web browser and go to http://localhost:9090 to access the Prometheus web interface.
    
<details>
  <summary>🔧 Installation Steps (Click to Expand)</summary>

  1. **Install Ansible on Ubuntu:** [Ansible Commands](https://docs.ansible.com/)
  2. **Install Jenkins on Ubuntu:** [Jenkins Guide](https://www.jenkins.io/)
  3. **Install Java:** `sudo apt install openjdk-17-jre`
  4. **Install Docker:** `sudo apt install docker.io`
  
</details>

