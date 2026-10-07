pipeline {
    agent any 
    options {
        timeout(time: 30, unit: 'MINUTES')
        timestamps()
        disableConcurrentBuilds()
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code from GitHub...'
                // Jenkins automatically clones your repo if this file is run via "Pipeline from SCM"
            }
        }

        stage('SAST - SonarQube Analysis') {
            steps {
                echo 'Starting SonarQube SAST scan...'
                script {
                    def scannerHome = tool 'SonarScanner'

                    withSonarQubeEnv('demo_sonarqube') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application to the server...'
            }
        }

        // stage('Clean Workspace') {
        //     steps {
        //         cleanWs()
        //     }
        // }
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
