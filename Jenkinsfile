




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
        script {
            // Run Trufflehog with all detectors enabled, concurrency optimizations, and a JSON output dump
            // We capture the status code so we can decide whether to break the build *after* archiving the artifact
            int scanStatus = sh(
                script: '''
                docker run --rm -v ${WORKSPACE}:/pwd trufflesecurity/trufflehog:latest filesystem \
                  --no-verification \
                  --include-detectors=all \
                  --concurrency=4 \
                  --json \
                  /pwd > ${WORKSPACE}/trufflehog-report.json
                ''',
                returnStatus: true
            )
            
            // Log the result status to the console log
            echo "Trufflehog scan completed with status code: ${scanStatus}"
            
            // If secrets were found (exit code 1), we can conditionally flag or fail the pipeline later
            if (scanStatus != 0) {
                echo "⚠️ WARNING: Potential secrets or hardcoded passwords discovered in the workspace!"
                // Uncomment the line below if you want the Jenkins build to explicitly FAIL when secrets are found:
                // error("Pipeline aborted due to secrets found during security scan.")
            }
        }
    }
    post {
        always {
            // Guarantees the JSON file is uploaded to the Jenkins UI for analysis regardless of scan success/failure
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
