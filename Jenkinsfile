pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the main branch explicitly
                    git branch: 'main', url: 'https://github.com/bcbigges/modern-web-app1.git'
                }
            }
        }

        // ... the rest of the pipeline stages
    }
}
