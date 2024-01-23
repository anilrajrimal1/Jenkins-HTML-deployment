
# Jenkins Installation Guide

This guide helps quickly install Jenkins on CentOS 7 for automating software development workflows. Ideal for system administrators and developers.
# Table of Contents

- [Installing Jenkins](#installing-jenkins)
  1. [Install Java](#1-install-java)
  2. [Set JAVA_HOME Variable Path](#2-set-java_home-variable-path)
  3. [Installing Jenkins](#3-installing-jenkins)
  4. [Start Jenkins Service](#4-start-jenkins-service)
  5. [Login to Jenkins Server](#5-login-to-jenkins-server)



## Installing Jenkins
### 1. Install Java

```bash
 # Install Java 11
sudo yum -y install java-11-openjdk

# Verify Java version
java -version

# Remove old version JDK (if necessary)
# OR configure alternative Java version
sudo alternatives --config java
java -version
```
### 2. Set JAVA_HOME Variable Path

```bash
# Find Java installation path
cd /usr/lib/jvm/java-11<tab>
JAVA_PATH=$(pwd)

# Edit bash profile
vi /root/.bash_profile

# Add the following lines
export JAVA_HOME=$JAVA_PATH
export PATH=$PATH:$HOME/bin:$JAVA_HOME

# Refresh the profile
source /root/.bash_profile

```
### 3. Installing Jenkins

```bash
# Navigate to YUM repository directory
cd /etc/yum.repos.d

# Browse the official Jenkins website for genuine repositories
# Replace <paste url for repo> with the actual repository URL
wget <paste url for repo>

# Replace <paste url for key> with the actual key URL
rpm --import <paste url for key>

# Install Jenkins
sudo yum -y install jenkins

# Verify Jenkins installation
rpm -q jenkins

```
### 4. Start Jenkins Service

```bash
# Start Jenkins service
sudo systemctl start jenkins

# Enable Jenkins service on startup
sudo systemctl enable jenkins

# Check Jenkins service status
sudo systemctl status jenkins
```
### 5. Login to Jenkins Server

- Default Jenkins port: 8080
- Obtain your server IP address using ifconfig
- goto browser and <your ip>:8080

```bash
# Follow the user-provided location to get the password

# Jenkins Dashboard
- Install suggested plugins
- Create the first admin user
  - Fill the form
  - Save and continue
  - Start using Jenkins! 😊
```
