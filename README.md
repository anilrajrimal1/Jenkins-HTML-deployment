# Project 1: CI/CD Setup with GitHub, Jenkins, and Apache Webserver
## Overview

This project aims to set up a Continuous Integration/Continuous Deployment (CI/CD) pipeline using GitHub, Jenkins, and an Apache Webserver. The main objectives include configuring Jenkins, Git, and Apache Webserver, integrating GitHub with Jenkins and Apache, creating CI/CD jobs, and testing the deployment of an artifact (http-based website).

developers --->push--> Git hub --->pull-->Jenkins -->pipeline - build --> webserver (Apache)
# Table of Contents

1. [Installing Apache server and php in webserver](#1-installing-apache-server-and-php-in-webserver)
2. [Creating index.html file in /var/www/html of webserver](#2-creating-indexhtml-file-in-varwwwhtml-of-webserver)
3. [Creating new repo in GitHub for our HTML project](#3-creating-new-repo-in-github-for-our-html-project)
4. [Jenkins configuration for "GitHub" and "Publish Over SSH"](#4-jenkins-configuration-for-github-and-publish-over-ssh)
5. [Jenkins and GitHub SSH authentication configuration](#5-jenkins-and-github-ssh-authentication-configuration)
6. [Set IP address, username, and password of webserver in Jenkins for automated file transfer (Publish Over SSH)](#6-set-ip-address-username-and-password-of-webserver-in-jenkins-for-automated-file-transfer-publish-over-ssh)
7. [Creating a job in Jenkins to pull index.html from GitHub repo and send it to the webserver into /var/www/html directory](#7-creating-a-job-in-jenkins-to-pull-indexhtml-from-github-repo-and-send-it-to-the-webserver-into-varwwwhtml-directory)

## 1. Installing Apache server and php in webserver
To set up the Apache server and PHP on your webserver, follow the steps below:

- Install Apache server and PHP:
   ```bash
   # Install Apache and PHP
   sudo yum -y install httpd php

    ```
![install-httpd](https://github.com/anilrajrimal1/myhtml/blob/master/screenshots/install%20httpd%20on%20Anil.png)

- Start the Apache service:
   ```bash
   # Start the Apache service
    sudo systemctl start httpd
   
    ```

- Enable the Apache service to start on boot:
   ```bash
   # Enable Apache service for startup
    sudo systemctl enable httpd 
    sudo systemctl status httpd
    ```
![status-httpd](https://github.com/anilrajrimal1/myhtml/blob/master/screenshots/httpd%20status.png)

- Add the HTTP service to the firewall (permanent):
   ```bash
   # Add HTTP port to firewall
    sudo firewall-cmd --permanent --add-service=http
    ```

- Reload the firewall to apply changes:
   ```bash
   # Reload the firewall
    sudo firewall-cmd --reload

    ```
![firewall](https://github.com/anilrajrimal1/myhtml/blob/master/screenshots/firewall%20port.png)
These commands will install Apache server and PHP, start the Apache service, enable it for startup, and configure the firewall to allow HTTP traffic.

## 2. Creating, Editing, and Deleting index.html File in /var/www/html of Webserver

To create, edit, and delete an `index.html` file on your webserver, follow the steps below:

### **Create and Edit index.html:**
   - Open the `index.html` file in the `/var/www/html` directory using a text editor (e.g., vi):
     ```bash
     # Open index.html file
     sudo vi /var/www/html/index.html
     ```
     - Add your HTML code to the file.
     - Save and exit the text editor.

### **Preview Changes:**
   - After editing the file, refresh your IP or hostname in your web browser to view the changes.

### **Delete index.html (Optional):**
   - If needed, you can remove the `index.html` file:
     ```bash
     # Remove index.html file
     sudo rm -f /var/www/html/index.html
     ```
     This command will delete the `index.html` file from the specified directory.

Remember to replace `<your code>` with your actual HTML code in the `index.html` file.

## 3. Creating New Repo in GitHub for Our HTML Project
To create a new GitHub repository for your HTML project and initialize it locally, follow the steps below:

### Login to GitHub:
   - Create a new repository named "myhtml".
   - Copy the repository URL.

- On your developer's machine, power on and create a folder for your project:
   ```bash
   # Create project workspace
   mkdir -p /root/project/myhtml
   cd /root/project/myhtml
   ```
- Initialize a local Git repository:
   ```bash
    git init
   ```
- Add the GitHub repository as the remote origin:
   ```bash
    git remote add origin <your repo url>

    # example:- git remote add origin git@github.com:anilrajrimal1/myhtml.git
   ```
- Pull the existing master branch from the GitHub repository:
   ```bash
    git pull origin master
   ```
- List the files in the project folder:
   ```bash
    ls
   ```
- Open the index.html file in a text editor and add HTML code:
   ```bash
    vi index.html

    # add your html code
   ```

![html-code](https://github.com/anilrajrimal1/myhtml/blob/master/screenshots/index%20on%20Developer.png)

- Add and commit changes:
   ```bash
    git add -A

    git commit -m "MyHTML first commit"
   ```
- Push changes to the GitHub repository:
   ```bash
    git push origin master
   ```

These commands create a new GitHub repository, set up a local Git repository, and push your HTML project to the GitHub repository.


## 4. Jenkins configuration for "GitHub" and "Publish Over SSH

To configure Jenkins with the "GitHub" and "Publish Over SSH" plugins, follow the steps below:

### Install Plugins for GitHub and Publish Over SSH:
   - Open the Jenkins dashboard.
   - Navigate to "Manage Jenkins" > "Manage Plugins."
   - In the "Available" tab, search for "GitHub Integration" and "Publish Over SSH."
   - Install both plugins without restarting Jenkins.
![github-plugin](https://github.com/anilrajrimal1/myhtml/blob/master/screenshots/github%20plugin.png)
![ssh-plugin](https://github.com/anilrajrimal1/myhtml/blob/master/screenshots/ssh%20plugin.png)

### Configure GitHub Integration:
- After installing the GitHub Integration plugin, navigate to the Jenkins dashboard.
- Go to "Manage Jenkins" > "Configure System."
- Look for the "GitHub" section.
- Enter the GitHub server and credentials information.


### Configure Publish Over SSH:
- After installing the Publish Over SSH plugin, navigate to the Jenkins dashboard.
- Go to "Manage Jenkins" > "Configure System."
- Look for the "Publish over SSH" section.
- Configure the SSH server details for automated file transfer.
![conf-ssh](https://github.com/anilrajrimal1/myhtml/blob/master/screenshots/ssh%20server%20added%20on%20jenkins.png)

Now, Jenkins is configured to integrate with GitHub, and the "Publish Over SSH" plugin is set up for automated file transfer to the webserver.

## 5. Jenkins and GitHub SSH authentication configuration

To configure SSH authentication between Jenkins and GitHub, follow the steps below:

### Generate SSH Key on Jenkins Master:
   - On the Jenkins master, open a terminal.
   - Run the following commands to generate an SSH key:
     ```bash
     # Generate SSH key
     ssh-keygen
     ```

### Retrieve the Public Key:
   - Change to the SSH key directory:
     ```bash
     cd /root/.ssh
     ```
   - Display the public key and copy it:
     ```bash
     cat <filename>.pub
     ```

### Add SSH Key to GitHub:
   - Copy the displayed key.
   - Add the key to your GitHub account in the "SSH and GPG keys" section.

### Test the SSH Connection:
   - Run the following command to test the connection to GitHub:
     ```bash
     ssh -T git@github.com
     ```

### Retrieve the Private Key:
   - Display the private key and copy it:
     ```bash
     cat <filenameof private key>
     ```

### Add SSH Credentials in Jenkins Dashboard:
   - Open the Jenkins dashboard.
   - Navigate to "Manage Jenkins" > "Manage Credentials."
   - Under "System," select "Global credentials."
   - Click on "Add Credentials."
     - Choose: SSH Username with private key.
     - Username: `<your username>`
     - Enter the private key directly: Click on "Add" and paste the copied private key.
     - Click "Create."

![add-credentials](https://github.com/anilrajrimal1/myhtml/blob/master/screenshots/global%20credential%20set.png)

Now, Jenkins is configured with SSH authentication for communication with GitHub.

## 6. Set IP address, username, and password of webserver in Jenkins for automated file transfer (Publish Over SSH)

To configure Jenkins for automated file transfer to the webserver using "Publish Over SSH," follow the steps below:

- Open the Jenkins dashboard.

- Navigate to "Manage Jenkins" > "Configure System."

#### Under "Publish over SSH," configure the SSH server details.

   ```markdown
   - Jenkins Dashboard
     --> Manage Jenkins
     --> Configure System
       - Publish Over SSH
         - SSH Servers
           - Add
             - Name: <name of your webserver>
             - Hostname: <IP address of webserver>
             - Username: <webserver username>
             - Check "Use password authentication, or use a different key."
               - Password: <webserver password>
```

- Click "Save" to apply the changes.

Now, Jenkins is configured with the necessary details to perform automated file transfer to the webserver using the "Publish Over SSH" plugin.


## 7. Creating a job in Jenkins to pull index.html from GitHub repo and send it to the webserver into /var/www/html directory

To create a Jenkins job for automating the process of pulling `index.html` from a GitHub repo and sending it to the webserver, follow the steps below:

### Open the Jenkins dashboard.

### Click on "New Item" to create a new job.

### Choose "Freestyle project" and provide a name for the job.

### Configure the Git repository details:
   - Under "Source Code Management," select "Git."
   - Enter the repository URL and provide the necessary credentials.

![Repo-job](https://github.com/anilrajrimal1/myhtml/blob/master/screenshots/added%20repo%20to%20job.png)

### Configure the Build Trigger:
   - Under "Build Triggers," select "Poll SCM."
   - Set the schedule to run the job using Cron syntax, e.g., `* * * * *` for polling every minute.
   - if you have no idea on Cron Syntax visit :- https://crontab.guru/

![build-trigger](https://github.com/anilrajrimal1/myhtml/blob/master/screenshots/set%20build%20trigger.png)

### Configure the Post-Build Action:
   - Under "Post-build Actions," select "Send build artifact over SSH."
     - Choose the SSH server previously configured (webserver).
     - Set the "Source files" to `index.html`.
     - Set the "Remote directory" to `/var/www/html`.
   ```markdown
   - Jenkins Dashboard
     --> New Item (New Job)
       - Project name: <Your Job Name>
       - Type: Freestyle project
       - Source Code Management
         - Git
           - Repository URL: <Your GitHub Repo URL> e.g git@github.com:anilrajrimal1/kanban-backend.git
           - Credentials: <Select appropriate credentials>
       - Build Triggers
         - Poll SCM
           - Schedule: * * * * * (for polling every minute)
       - Post-build Actions
         - Send build artifact over SSH
           - SSH Server: webserver (Select server name)
           - Source files: index.html
           - Remote directory: /var/www/html
```
![post-build](https://github.com/anilrajrimal1/myhtml/blob/master/screenshots/post%20build%20action.png)

- Click "Save" to create the Jenkins job.

Now, Jenkins is configured to pull index.html from the GitHub repo and send it to the webserver's /var/www/html directory after each build.

### Console output and Result on Webserver
![Console-output](https://github.com/anilrajrimal1/myhtml/blob/master/screenshots/Build%20console%20output.png)

![Anil-Result](https://github.com/anilrajrimal1/myhtml/blob/master/screenshots/Deply%20on%20Anil-User.png)
