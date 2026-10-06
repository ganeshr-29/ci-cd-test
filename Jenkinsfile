




pipeline {
    agent any 
    options {
        timeout(time: 30, unit: 'MINUTES')
        timestamps()
        disableConcurrentBuilds()
    }

    stages {
        // stage('Clean Workspace') {
        //     steps {
        //         cleanWs()
        //     }
        // }

        stage('Trufflehog Secret Scan') {
            steps {
                // Runs Trufflehog container against the checked-out workspace folder
                // Generates a JSON report for Jenkins to archive
                sh '''
                docker run --rm -v ${WORKSPACE}:/pwd trufflesecurity/trufflehog:latest filesystem /pwd --only-verified=false --json > ${WORKSPACE}/trufflehog-report.json || true
                '''
            }
            post {
                always {
                    // This archives the report file so you can see it on your Jenkins build page
                    archiveArtifacts artifacts: 'trufflehog-report.json', allowEmptyArchive: true
                }
            }
        }

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
