pipeline {
    agent any

    tools {
        jdk 'jdk17'       // Make sure JDK 17 is installed in Jenkins global tools
        maven 'maven3'    // Likewise, install Maven under "Global Tool Configuration"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'github-token', url: 'https://github.com/Saravanank2005/model_lab.git'
            }
        }

        stage('Build') {
            steps {
                echo '🏗️ Building the Banking API...'
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Unit Tests') {
            steps {
                echo '🧪 Running unit tests...'
                sh 'mvn test'
                junit 'target/surefire-reports/*.xml'
            }
        }

        stage('Security Validation') {
            steps {
                echo '🔒 Running OWASP Dependency Check...'
                sh 'mvn org.owasp:dependency-check-maven:check'
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying locally...'
                // Replace with your start command if needed
                sh '''
                if pgrep -f "java -jar target" > /dev/null; then
                    echo "Stopping existing app..."
                    pkill -f "java -jar target" || true
                fi
                nohup java -jar target/*.jar > app.log 2>&1 &
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Build, Test, Security Scan, and Deploy completed successfully!'
        }
        failure {
            echo '❌ Build failed. Check Jenkins console output.'
        }
    }
}
