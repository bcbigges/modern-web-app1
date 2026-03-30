pipeline {
    agent any
    
    environment {
        PYTHON_VERSION = "python3"
    }
    
    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the code from GitHub
                    git branch: 'main', url: 'https://github.com/bcbigges/modern-web-app1.git'
                }
            }
        }

        stage('Setup Python Environment') {
            steps {
                script {
                    // Install python3-venv if not installed (Debian/Ubuntu systems)
                    sh ''' 
                    if ! dpkg -l | grep -q python3-venv; then
                        echo "python3-venv not found. Installing..."
                        sudo apt-get update
                        sudo apt-get install -y python3-venv
                    fi
                    '''
                    
                    // Create and activate the virtual environment
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
                    // Install dependencies using pip
                    sh ''' 
                    source venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt  # Ensure you have a requirements.txt
                    '''
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run tests using pytest
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
                    // Deactivate the virtual environment
                    sh 'deactivate'
                }
            }
        }
    }

    post {
        always {
            // Clean up workspace after the build completes
            cleanWs()
        }
        success {
            // Optionally, you can send a notification or log something if the build is successful
            echo 'Build completed successfully!'
        }
        failure {
            // Optionally, you can send a notification or log something if the build fails
            echo 'Build failed!'
        }
    }
}
