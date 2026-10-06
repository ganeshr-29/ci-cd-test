pipeline {
    agent any 

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code from GitHub...'
                // Jenkins automatically clones your repo if this file is run via "Pipeline from SCM"
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application...'
                // Example for Node.js: sh 'npm install'
                // Example for Java/Maven: sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                echo 'Running automated tests...'
                // Example: sh 'npm test' or sh 'mvn test'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application to the server...'
            }
        }
    }

    post {
        always {
            echo 'Pipeline has finished executing.'
        }
        success {
            echo 'Build completed successfully! 🎉'
        }
        failure {
            echo 'Build failed. Please check the logs. ❌'
        }
    }
}
