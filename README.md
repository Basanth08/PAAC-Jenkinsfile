![Everything Needs Automation](Diagrams/intro.png)

# Everything Needs Automation

In today's fast-paced software development world, automation is not just a luxury—it's a necessity. Manual processes are error-prone, slow, and difficult to scale. By automating repetitive tasks, teams can focus on innovation and deliver value faster.

**Pipeline as Code** is a key enabler of automation in CI/CD workflows. It allows you to:

- **Automate Build, Test, and Deployment:** Define every step of your software delivery process in code, ensuring consistency and reliability.
- **Reduce Human Error:** Automated pipelines minimize the risk of mistakes that can occur with manual interventions.
- **Accelerate Delivery:** Automation speeds up the feedback loop, enabling rapid iteration and faster releases.
- **Scale Effortlessly:** As your project grows, automated pipelines can easily adapt to handle more complex workflows and larger teams.

> **Embrace automation—let your pipelines do the heavy lifting, so you can focus on what matters most: building great software.**

# Pipeline as a Code

## Overview

This project demonstrates the concept of "Pipeline as a Code"—a practice where data or software pipelines are defined, managed, and versioned using code. This approach enables automation, reproducibility, and collaboration for building and maintaining complex workflows.

## Features
- Define pipelines programmatically
- Version control for pipeline definitions
- Automation and reproducibility

## Introduction

- Automate pipeline setup with Jenkinsfile
- Jenkinsfile defines Stages in CI/CD Pipeline
- Jenkinsfile is a text file with Pipeline DSL Syntax
- Syntax is similar to Groovy
- Two Syntax options:
  - Scripted
  - Declarative

## Getting Started

1. **Clone the repository:**
   ```bash
   git clone <repo-url>
   ```
2. **Navigate to the project directory:**
   ```bash
   cd PAAC-Jenkinsfile
   ```
3. **Install dependencies:**
   (Add instructions here for your stack, e.g., Python, Node.js, etc.)

## Usage

(Provide examples or instructions on how to use or run the pipeline code.)


## Pipeline Concepts

- **Pipeline**
- **Node/Agent**
- **Stage**
- **Step**

## Sample Jenkinsfile

```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                // Build steps go here
            }
        }
        stage('Test') {
            steps {
                // Test steps go here
            }
        }
        stage('Deploy') {
            steps {
                // Deploy steps go here
            }
        }
    }
}
```

## Jenkins Pipeline Structure Examples

### Basic Pipeline Structure
```groovy
pipeline {
    agent {
        // agent configuration
    }
    tools {
        // tool configuration
    }
    environment {
        // environment variables
    }
    stages {
        // stages go here
    }
}
```

### Agent and Tools Example
```groovy
pipeline {
    agent {
        label "master"
    }
    tools {
        maven "Maven"
    }
}
```

### Environment Example
```groovy
pipeline {
    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "you-ip-addr-here:8081"
        NEXUS_REPOSITORY = "maven-nexus-repo"
        NEXUS_CREDENTIAL_ID = "nexus-user-credentials"
        ARTVERSION = "${env.BUILD_ID}"
    }
}
```

### Stages Example
```groovy
pipeline {
    stages {
        stage("Clone code from VCS") {
            // steps go here
        }
        stage("Maven Build") {
            // steps go here
        }
        stage("Publish to Nexus Repository Manager") {
            // steps go here
        }
    }
}
```

### Stage with Steps and Post
```groovy
pipeline {
    stages {
        stage("Clone code from VCS") {
            steps {
                // steps go here
            }
            post {
                // post actions go here
            }
        }
    }
}

### Stage with Steps and Post-Success Example
```groovy
pipeline {
    stage('BuildAndTest') {
        steps {
            sh 'mvn install'
        }
        post {
            success {
                echo 'Now Archiving...'
                archiveArtifacts artifacts: '**/target/*.war'
            }
        }
    }
}

## 🖥️ Jenkins Server Setup (Ubuntu)

Easily set up a Jenkins server on Ubuntu with the following steps and script:

### 1. Install Jenkins & Java 17

```bash
#!/bin/bash
sudo apt update
sudo apt install -y openjdk-17-jdk ca-certificates apt-transport-https wget

sudo mkdir -p /etc/apt/keyrings
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | \
  sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update
sudo apt-get install -y jenkins

sudo systemctl start jenkins
sudo systemctl enable jenkins

# Optional: Open firewall port
# sudo ufw allow 8080

sudo systemctl status jenkins
```

### 2. Troubleshooting & Tips

- **Check Jenkins status:**
  ```bash
  sudo systemctl status jenkins
  ```
- **Check logs for errors:**
  ```bash
  sudo journalctl -u jenkins --no-pager | tail -50
  ```
- **Verify Java version:**
  ```bash
  java -version
  # Should show Java 17
  ```
- **Set Java 17 as default (if needed):**
  ```bash
  sudo update-alternatives --config java
  ```
- **Open port 8080 in your cloud firewall/security group.**
- **Get the initial admin password:**
  ```bash
  sudo cat /var/lib/jenkins/secrets/initialAdminPassword
  ```

## ☕ Java Version Management

Managing multiple Java versions is a common DevOps task. Here's how to list and switch Java versions on both Ubuntu and macOS:

### On Ubuntu/Linux

**List installed Java versions:**
```bash
ls /usr/lib/jvm
```

**Set JAVA_HOME for a specific version (e.g., Java 8):**
```bash
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH
```

**Check the active Java version:**
```bash
java -version
```

**Switch default Java version system-wide:**
```bash
sudo update-alternatives --config java
```

### On macOS

**List installed Java versions:**
```bash
/usr/libexec/java_home -V
```

**Set JAVA_HOME for a specific version (e.g., Java 8 or 17):**
```bash
export JAVA_HOME=$(/usr/libexec/java_home -v 1.8)   # for Java 8
export JAVA_HOME=$(/usr/libexec/java_home -v 17)    # for Java 17
export PATH=$JAVA_HOME/bin:$PATH
```

**Check the active Java version:**
```bash
java -version
```

This section demonstrates practical DevOps skills for cross-platform Java management—essential for CI/CD, Jenkins, and automation workflows.

## 🔌 Recommended Jenkins Plugins

Enhance your Jenkins pipelines with these essential plugins:

### 1. Pipeline Maven Integration
- **Purpose:** Seamlessly integrates Maven builds into Jenkins pipelines.
- **Benefits:**
  - Automatic detection and management of Maven build steps
  - Advanced reporting for tests, code coverage, and static analysis
  - Dependency caching for faster builds
  - Visualizes Maven build stages in Jenkins UI

### 2. Pipeline Utility Steps
- **Purpose:** Adds a collection of utility steps for use in Jenkins pipelines.
- **Benefits:**
  - Read/write files, manipulate archives, parse JSON/YAML, and more
  - Enables advanced scripting and automation in Jenkinsfiles
  - Makes pipelines more flexible and powerful

### 🧩 Example Usage in Jenkinsfile
```groovy
pipeline {
    agent any
    stages {
        stage('Build with Maven') {
            steps {
                // Uses the Maven integration plugin
                withMaven(maven: 'Maven 3.8.1') {
                    sh 'mvn clean install'
                }
            }
        }
        stage('Read a File') {
            steps {
                // Uses the utility steps plugin
                script {
                    def content = readFile 'pom.xml'
                    echo "POM file content: ${content.substring(0, 100)}"
                }
            }
        }
    }
}
```

These plugins make your Jenkins pipelines more maintainable, efficient, and production-ready—demonstrating your ability to build real-world DevOps solutions.

## 🚨 Jenkinsfile Troubleshooting Guide

### Common Issues and Solutions

#### 1. Maven Not Found Error (`mvn: not found`)

**Problem:**
```
[Pipeline] sh
+ mvn clean install
/var/lib/jenkins/workspace/sample-paac@tmp/durable-57effa3b/script.sh.copy: 1: mvn: not found
ERROR: script returned exit code 127
```

**Solutions:**

**Option A: Configure Maven in Jenkins (Recommended)**
1. Go to Jenkins Dashboard → Manage Jenkins → Tools
2. Find "Maven installations" section
3. Add a new Maven installation:
   - Name: `Maven`
   - Install automatically: Check this box
   - Version: Choose a version (e.g., 3.9.5)

**Option B: Use Tools Directive in Jenkinsfile**
```groovy
pipeline {
    agent any
    
    tools {
        maven 'Maven'  // Must match the name configured in Jenkins
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
    }
}
```

**Option C: Local Maven Installation (No Jenkins Config Required)**
```groovy
pipeline {
    agent any
    
    stages {
        stage('Install Maven') {
            steps {
                sh '''
                    # Check if Maven is already installed
                    if ! command -v mvn &> /dev/null; then
                        echo "Installing Maven locally..."
                        # Download Maven
                        wget https://archive.apache.org/dist/maven/maven-3/3.9.5/binaries/apache-maven-3.9.5-bin.tar.gz
                        # Extract Maven
                        tar -xzf apache-maven-3.9.5-bin.tar.gz
                        # Add Maven to PATH for this session
                        export PATH=$PWD/apache-maven-3.9.5/bin:$PATH
                        echo "Maven installed successfully"
                        mvn --version
                    else
                        echo "Maven is already installed"
                        mvn --version
                    fi
                '''
            }
        }
        stage('Build') {
            steps {
                sh '''
                    # Set Maven path if installed locally
                    if [ -d "apache-maven-3.9.5" ]; then
                        export PATH=$PWD/apache-maven-3.9.5/bin:$PATH
                    fi
                    mvn clean install
                '''
            }
        }
    }
}
```

#### 2. Sudo Password Required Error

**Problem:**
```
sudo: a terminal is required to read the password; either use the -S option to read from standard input or configure an askpass helper
sudo: a password is required
```

**Solution:** Use the local Maven installation approach (Option C above) which doesn't require sudo privileges.

#### 3. Git Credentials Issues

**Problem:**
```
The recommended git tool is: NONE
No credentials specified
```

**Solutions:**
1. **For Public Repositories:** This is normal and safe to ignore
2. **For Private Repositories:** Configure credentials in Jenkins:
   - Go to Jenkins Dashboard → Manage Jenkins → Credentials
   - Add credentials (Username/Password or SSH Key)
   - Use credentials in Jenkinsfile:
   ```groovy
   stage('Fetch code') {
       steps {
           git branch: 'paac', 
                url: 'https://github.com/devopshydclub/vprofile-project.git',
                credentialsId: 'your-credential-id'
       }
   }
   ```

#### 4. Java Version Issues

**Problem:**
```
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-compiler-plugin:3.8.1:compile
[ERROR] Fatal error compiling: invalid target release: 17
```

**Solution:** Ensure the correct Java version is available:
```groovy
pipeline {
    agent any
    
    tools {
        maven 'Maven'
        jdk 'JDK 17'  // Specify the correct JDK version
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
    }
}
```

### Complete Working Jenkinsfile Example

```groovy
pipeline {
    agent any
    
    stages {
        stage('Fetch code') {
            steps {
                git branch: 'paac', url: 'https://github.com/devopshydclub/vprofile-project.git'
            }
        }
        stage('Install Maven') {
            steps {
                sh '''
                    # Check if Maven is already installed
                    if ! command -v mvn &> /dev/null; then
                        echo "Installing Maven locally..."
                        # Download Maven
                        wget https://archive.apache.org/dist/maven/maven-3/3.9.5/binaries/apache-maven-3.9.5-bin.tar.gz
                        # Extract Maven
                        tar -xzf apache-maven-3.9.5-bin.tar.gz
                        # Add Maven to PATH for this session
                        export PATH=$PWD/apache-maven-3.9.5/bin:$PATH
                        echo "Maven installed successfully"
                        mvn --version
                    else
                        echo "Maven is already installed"
                        mvn --version
                    fi
                '''
            }
        }
        stage('Build') {
            steps {
                sh '''
                    # Set Maven path if installed locally
                    if [ -d "apache-maven-3.9.5" ]; then
                        export PATH=$PWD/apache-maven-3.9.5/bin:$PATH
                    fi
                    mvn clean install
                '''
            }
        }
        stage('Test') {
            steps {
                sh '''
                    # Set Maven path if installed locally
                    if [ -d "apache-maven-3.9.5" ]; then
                        export PATH=$PWD/apache-maven-3.9.5/bin:$PATH
                    fi
                    mvn test
                '''
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline completed'
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
```

### Best Practices

1. **Always check prerequisites** before running build commands
2. **Use local installations** when Jenkins tools aren't configured
3. **Add proper error handling** with post actions
4. **Test your pipeline** with a simple project first
5. **Keep Jenkinsfile version controlled** alongside your code
6. **Use declarative syntax** for better readability and maintainability

This troubleshooting guide covers the most common issues you'll encounter when setting up Jenkins pipelines for Java/Maven projects. 