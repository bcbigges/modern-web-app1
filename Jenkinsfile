pipeline {
    agent any
    
    environment {
        // Optional: Define the Python version if needed
        PYTHON_VERSION = "python3"
    }
    
    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout latest code from GitHub
                    git 'https://github.com/bcbigges/modern-web-app1.git'
                }
            }
        }

        stage('Setup Python Environment') {
            steps {
                script {
                    // Create and activate a virtual environment
                    sh ''' 
                    ${PYTHON_VERSION} -m venv venv
                    source venv/bin/activate
                    '''
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Install necessary Python packages
                    sh ''' 
                    source venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt  # if you have a requirements.txt
                    '''
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Optionally, run the tests with pytest
                    sh ''' 
                    source venv/bin/activate
                    pytest tests/ --maxfail=1 --disable-warnings -q  # Adjust based on your test directory
                    '''
                }
            }
        }
        
        stage('Cleanup') {
            steps {
                script {
                    // Deactivate and clean up if needed
                    sh 'deactivate'
                }
            }
        }
    }

    post {
        always {
            // Clean up if the job fails or succeeds
            cleanWs()
        }
    }
}