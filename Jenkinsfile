pipeline {
    agent any
    
    environment {
        // Define Python version and virtual environment path
        PYTHON_VERSION = "python3"
        VENV_DIR = "venv"
    }
    
    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the latest code from GitHub
                    git branch: 'main', url: 'https://github.com/bcbigges/modern-web-app1.git'
                }
            }
        }

        stage('Setup Python Environment') {
            steps {
                script {
                    // Create and activate virtual environment
                    sh '''
                        if ! python3 -m venv --help; then
                            echo "Python venv module is missing. Skipping Python environment setup."
                            exit 1
                        fi
                        if [ ! -d "venv" ]; then
                            ${PYTHON_VERSION} -m venv ${VENV_DIR}
                        fi
                        source ${VENV_DIR}/bin/activate
                    '''
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Install Python dependencies
                    sh '''
                        source ${VENV_DIR}/bin/activate
                        pip install --upgrade pip
                        if [ -f "requirements.txt" ]; then
                            pip install -r requirements.txt
                        else
                            echo "No requirements.txt found. Skipping pip install."
                        fi
                    '''
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run tests using pytest (or other test framework)
                    sh '''
                        source ${VENV_DIR}/bin/activate
                        if [ -d "tests" ]; then
                            pytest tests/ --maxfail=1 --disable-warnings -q
                        else
                            echo "No tests directory found. Skipping test execution."
                        fi
                    '''
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    // Clean up environment after the build
                    sh '''
                        deactivate || true
                        rm -rf ${VENV_DIR}
                    '''
                }
            }
        }
    }

    post {
        always {
            // Cleanup workspace after the job finishes
            cleanWs()
        }
    }
}
