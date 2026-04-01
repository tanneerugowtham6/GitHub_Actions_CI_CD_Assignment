# GitHub_Actions_CI_CD_Assignment

---

## Project Overview

This repository demonstrates the implementation of a complete Continuous Integration and Continuous Deployment (CI/CD) pipeline using Jenkins for a Python Flask-based Student Registration System application.

The pipeline automates the entire software delivery lifecycle, including environment setup, dependency installation, application testing, and deployment to a staging server hosted on AWS EC2. The deployed application is managed as a background service using systemd, ensuring high availability, automatic restarts, and reliability.

Additionally, the pipeline is integrated with GitHub webhooks to enable automatic build triggers on code changes pushed to the main branch, enabling a fully automated and efficient deployment workflow.

---

## Project Execution

This project is executed in **4 phases**, each containing a set of clear deployment tasks required to fully host the Python Flask-based Student Registration System application on AWS.

### Phases of Deployment

- **Phase 1:** Environment & Source Control Setup
- **Phase 2:** Writing the Jenkinsfile
- **Phase 3:** Configure the service for Flask Application
- **Phase 4:** Jenkins Job Configuration

---

## Environment

### Cloud Platform
- **Provider:** AWS EC2

### Operating System
- **OS:** Ubuntu 20.04 LTS

### AWS Services Used
- EC2
- Security Groups
- VPC

### Database
- MongoDB Atlas

## Technology Stack
- Python
- Jenkins
- Git

---

## Phase 1: Repository Setup for Flask Application 

### Task-1: Create a Git Repository for Flask Application

1. Create a Repository in GitHub
2. Clone it to the local using the below command

    ```
    git clone <repo-url>
    ```
    
3. Add the Flask application code to the folder
4. Add and commit the code to remote repository

    ```
    git add .
    git commit -m "<your-messsage>"
    git push origin main
    ```
    
5. Create the staging branch in local and push to the remote repository

    ```
    git checkout -b staging
    git push origin staging
    ```

---

## Phase 2: GitHub Actions CI (Build + Test)

### Task-1: Create workflows

1. Switch to the main branch in local repository

    ```
    git checkout main
    ```
    
2. Create `.github/workflows` folder

    ```
    mkdir -p .github/workflows
    ```

    <img width="712" height="37" alt="image" src="https://github.com/user-attachments/assets/6766f16d-3f76-4697-9f1a-8294633469a0" />

3. Create a yaml file, [Refer to the flask_ci.yml file in this repository]
4. Add, commit and push the changes to main branch

    <img width="758" height="235" alt="image" src="https://github.com/user-attachments/assets/3e0c3cd2-5503-4de8-84a5-0936da734c8b" />
    
### Task-2: Verify the pipeline

1. Go to GitHub Actions Dashboard, by navigating to the remote repository in GitHub
2. Then click on Actions

    <img width="1710" height="490" alt="image" src="https://github.com/user-attachments/assets/ad5b9571-5194-4139-9760-163ed7293d55" />

3. Wokflow should be triggered
4. Go to GitHub Repository, click on **Settings**

    <img width="878" height="151" alt="image" src="https://github.com/user-attachments/assets/0ac20de1-c391-4ed8-bfe1-00905bbde120" />

5. From the left side menu expand **Secrets and variables**, select **Actions**

    <img width="1363" height="821" alt="image" src="https://github.com/user-attachments/assets/e3fe5f5c-8a50-4c0b-bde2-00f2877645b5" />

6. Click on New Repository Secret, add **Name** and **Secret**

    <img width="1386" height="553" alt="image" src="https://github.com/user-attachments/assets/6688ce5b-f37b-41c6-b73b-76b60065cec5" />

7.  Click on Add secret
8.  Repeat the same to add another secrets

> [!NOTE]
> This workflow requires to secret vairables, MONGO_URI, SECRET_KEY to be passed for the application.

9.  Commit the any pending chnages to the repository, which triggers the workflow

    <img width="1658" height="337" alt="image" src="https://github.com/user-attachments/assets/8fc7f982-b1b8-4fd9-9642-2fcfa47b70e0" />

10. Once the pipeline runs successfully and all teh conflicts were resolved, merge the changes to `main` branch

    ```
    git checkout main
    git merge staging
    git push origin main
    ```

    <img width="662" height="117" alt="image" src="https://github.com/user-attachments/assets/685c568b-bd97-403a-b976-604df5974372" />
    <img width="662" height="159" alt="image" src="https://github.com/user-attachments/assets/7af08731-0565-4839-8d69-07aed8c95d35" />
    
11. zxz
12. xc

---

## Phase 3: Prepare the Staging and Production Infrastructure

### Task-1: Launching EC2 Instances

#### Steps:

1. Login to your AWS Account and search for EC2 service

    <img width="949" height="187" alt="image" src="https://github.com/user-attachments/assets/c899ca60-e19f-4ba9-9e5c-30015013e660" />

2. Click on `Launch Instance`

    <img width="297" height="138" alt="image" src="https://github.com/user-attachments/assets/bdcf3af9-b39b-49dc-9535-0809f7018e24" />

3. Enter the name of the instance, select `Ubuntu Server 24.04 LTS` AMI and select the type of the instance

    <img width="881" height="581" alt="image" src="https://github.com/user-attachments/assets/fd775572-1e93-496f-8507-ba911dafd5c2" />

4. Create a new keypair (Optional) or choose an existing keypair

    <img width="494" height="472" alt="image" src="https://github.com/user-attachments/assets/f62ded24-64f3-4713-bbaa-30cec9b1227a" />

5. Under Networks Settings, click on `edit`. Select the required VPC and a Public Subnet

    <img width="785" height="273" alt="image" src="https://github.com/user-attachments/assets/9b0c4bfe-0569-4345-bc9f-6d4044cb189d" />

6. Under the **Firewall (security groups)**, select **Create security group**. Enter the required details, **Security group name, Description** or use the existing **Securtiy group** with the required **Inbound rules**

    <img width="872" height="816" alt="image" src="https://github.com/user-attachments/assets/8d8cb150-df91-4e7e-9511-da869fcc88ad" />

7. Allow the ports `80, 443, 22` or `HTTP, HTTPS, SSH` as below using **Add security group rule** button

    <img width="795" height="820" alt="image" src="https://github.com/user-attachments/assets/602e22a2-21f1-4578-a5ff-3319bda2d0e9" />

8. Click on the `Launch instance` button, which initiates the instance. Refer below for the confirmation

    <img width="622" height="182" alt="image" src="https://github.com/user-attachments/assets/e40b6b0b-554c-4d5d-ba0f-196cf4d032c5" />

9. Repeat the same process to create an EC2 instance for Staging Environment

### Task-2: Prepare the environment

#### Steps:

1. Go to the Staging EC2 Instance and copy the Public IPv4 or Public DNS
2. Launch the Terminal (macOS), Command Prompt (Windows)
3. Navigate to the folder where your `.pem` file is downloaded and assign the executable permissions to the file

    ```sh
    cd <.pem_file_location>
    chmod 700 skilltestkeypair.pem
    ```

    <img width="579" height="68" alt="image" src="https://github.com/user-attachments/assets/3c9db7a4-9f2d-4eba-b9db-65ea9973c85b" />

4. Connect to the EC2 Instance using the `ssh` command

    ```sh
    ssh -i "<.pem_file_name>" <username>@<public_dns or ipv4 of the instance>
    ```

    <img width="906" height="643" alt="image" src="https://github.com/user-attachments/assets/e0268094-098a-4611-9739-0c17a4d1c678" />
    
5. Update and upgrade the available packages

    ```sh
    sudo apt update
    sudo apt upgrade -y
    ```

    <img width="853" height="953" alt="image" src="https://github.com/user-attachments/assets/70609570-2cb6-4921-b8e3-b89b1bf6d805" />
    <img width="1312" height="313" alt="image" src="https://github.com/user-attachments/assets/a47e70ea-b596-4464-8527-c78a7b7440e1" />
    <img width="817" height="365" alt="image" src="https://github.com/user-attachments/assets/002d9cac-1a41-46a9-92e5-510453322056" />

6. Install required tools or software, like Python, which is required for our Flask application

    ```sh
    sudo apt install python3 python3-pip -y
    ```

    <img width="1546" height="397" alt="image" src="https://github.com/user-attachments/assets/5226706d-9a8d-4fa5-a4c7-acf852a87e78" />
    <img width="820" height="397" alt="image" src="https://github.com/user-attachments/assets/5549aa81-7f98-4750-8dd3-810a57145e50" />

7. Verify the installations

    ```sh
    python3 --version
    pip3 --version
    ```

    <img width="450" height="76" alt="image" src="https://github.com/user-attachments/assets/cb857d17-26a0-4b6f-ad1d-0687b1a207f4" />

8. Install Python Virtual Environment

    ```
    sudo apt install python3.12-venv -y
    python3 -m venv venv
    source venv/bin/activate
    ```

**9. Install Application dependencies (flask and gunicorn)

    ```
    pip install flask gunicorn
    ```
    
10. Run the application

    ```
    gunicorn -w 4 app:app -b 0.0.0.0:5000
    ```**
    
11. Make sure the below folder structure in the EC2 instance, if `app` folder is missing, create it

    ```
    /home/ubuntu/app
    ```
    
12. Repeat the same steps for Production EC2 Instance

### Task-3: Create systemd Service

### Steps:

1. Connect to the Staging EC2 Instance, create `flask-app.service` file

    ```sh
    sudo nano /etc/systemd/system/flask-app.service
    ```
    
2. Paste the below configuration to the service file

    ```sh
    [Unit]
    Description=Python Web App
    After=network.target
    
    [Service]
    User=ubuntu
    WorkingDirectory=/home/ubuntu/App
    ExecStart=/home/ubuntu/App/venv/bin/python app.py
    Restart=always
    EnvironmentFile=/home/ubuntu/App/.env
    
    [Install]
    WantedBy=multi-user.target
    ```
    
3. Execute the below commands to start the service

    ```sh
    sudo systemctl daemon-reexec
    sudo systemctl daemon-reload
    sudo systemctl enable flask-app
    sudo systemctl start flask-app
    ```

    <img width="796" height="80" alt="image" src="https://github.com/user-attachments/assets/d7215ce4-5b15-4c39-8b4a-7f8c2f9b6ff8" />

4. Verify the service

    ```sh
    sudo systemctl status flask-app
    ```

    <img width="1172" height="349" alt="image" src="https://github.com/user-attachments/assets/c70655df-7c4b-4258-a6bd-f4a352cffddb" />

---

## Phase 4: Deploying to Staging Environment

### Task-1:

#### Steps:

1. Switch to `staging` branch and add the below script to the `flask_ci.yml` pipeline

    ```sh
    - name: Configuring ssh-agent
        uses: webfactory/ssh-agent@v0.8.0
        with:
          ssh-private-key: ${{ secrets.EC2_SSH_KEY }}
    
    - name: Add EC2 to known hosts
        run: |
          mkdir -p ~/.ssh
          ssh-keyscan -H 3.238.90.48 >> ~/.ssh/known_hosts # Add Staging EC2 host key to known hosts
          ssh-keyscan -H 3.235.176.224 >> ~/.ssh/known_hosts # Add Production EC2 host key to known hosts
    
    - name: Deploy to Staging environment
        run: |
          ssh ubuntu@3.238.90.48 << 'EOF'
            cd /home/ubuntu
            if [ ! -d "app"] then
              git clone https://github.com/tanneerugowtham6/GitHub_Actions_CI_CD_Assignment.git
            fi
            cd app/GitHub_Actions_CI_CD_Assignment
            git pull origin staging
            pip3 install -r requirements.txt
            sudo systemctl restart flask-app
          EOF
    ```
2. Save and commit the changes to the local repository
3. Go to GitHub Repository, click on **Settings**

    <img width="878" height="151" alt="image" src="https://github.com/user-attachments/assets/0ac20de1-c391-4ed8-bfe1-00905bbde120" />

4. From the left side menu expand **Secrets and variables**, select **Actions**

    <img width="1363" height="821" alt="image" src="https://github.com/user-attachments/assets/e3fe5f5c-8a50-4c0b-bde2-00f2877645b5" />

5. Click on New Repository Secret, add **Name** and **Secret**

    <img width="1386" height="553" alt="image" src="https://github.com/user-attachments/assets/6688ce5b-f37b-41c6-b73b-76b60065cec5" />

6.  Click on Add secret
7.  s
8.  sd
9.  cv
10.  sdc
11.  as

---
