pipeline {
    agent any 
    options {
        timeout(time: 30, unit: 'MINUTES')
        timestamps()
        disableConcurrentBuilds()
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Gitleaks Secret Scan') {
            steps {
                sh '''
                    # Run Gitleaks in a container against the current Jenkins workspace
                    # --redact: masks sensitive keys in console logs
                    # --report-path: saves full findings for review
                    # Exit code 1 indicates leaked secrets (fails the stage)
                    docker run --rm \
                        -v "${WORKSPACE}:/repo" \
                        zricethezav/gitleaks:v8.18.2 detect \
                        --source="/repo" \
                        --verbose \
                        --redact \
                        --report-format=json \
                        --report-path="/repo/gitleaks-report.json"
                '''
            }
            post {
                always {
                    // Archive the findings report as a Jenkins build artifact
                    archiveArtifacts artifacts: 'gitleaks-report.json', allowEmptyArchive: true
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
}
