pipeline {
    agent any
    stages {
        stage('PR Validation') {
            steps {
                echo "Validating Pull Request #${env.CHANGE_ID}"
                echo "Source: ${env.CHANGE_BRANCH} -> Target: ${env.CHANGE_TARGET}"
                // Add your build/test commands here (e.g., sh 'npm test')
            }
        }
    }
}
