pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout the latest code from GitHub
                git branch: 'main', url: 'https://github.com/bcbigges/modern-web-app1.git'
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run tests (replace with your specific test command)
                    sh 'pytest tests/ --maxfail=1 --disable-warnings -q'
                }
            }
        }

        stage('Cleanup') {
            steps {
                // Clean up after the test execution (if needed)
                cleanWs()
            }
        }
    }

    post {
        always {
            // Ensure workspace is cleaned up after execution
            cleanWs()
        }
    }
}
