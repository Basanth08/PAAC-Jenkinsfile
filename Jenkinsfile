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
}