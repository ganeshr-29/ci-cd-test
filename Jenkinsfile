// @Library('devops-shared-pipeline') _
// runProductionWorkflow()



pipeline {
        agent any

    options {
        disableConcurrentBuilds()
        timeout(time: 1, unit: 'HOURS')
    }

    stages {
        // SCENARIO 1: Developer creates a PR targeting the dev branch
        stage('PR Validation (dev)') {
            when {
                expression { env.CHANGE_TARGET == 'dev' }
            }
            steps {
                echo "🔨 Validating Pull Request #${env.CHANGE_ID}"
                echo "Running code tests, linters, and security scans..."
                // Example: sh 'npm run test' or 'mvn test'
            }
        }

        // SCENARIO 2: Merged changes pushed/merged into the prod branch
        stage('Deploy to Production') {
            when {
                branch 'prod'
            }
            steps {
                echo "🚀 Deploying to Production environment..."
                // Example: sh './deploy-prod.sh'
            }
        }
    }

    post {
        success {
            echo "✅ Stage executed successfully. Notifying Git provider."
        }
        failure {
            echo "❌ Pipeline failed. Please check Jenkins logs."
        }
    }
}