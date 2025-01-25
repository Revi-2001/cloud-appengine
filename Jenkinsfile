pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/Revi-2001/cloud-appengine.git', branch: 'main'
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                    sh '''
                    # Use bash to run the commands
                    /bin/bash -c "
                    # Create a virtual environment
                    python3 -m venv venv
                    source venv/bin/activate
                    
                    # Upgrade pip
                    pip install --upgrade pip
                    
                    # Install Poetry globally
                    curl -sS https://install.python-poetry.org | python3 -
                    
                    # Ensure Poetry is in the correct PATH for this session
                    export PATH=\$HOME/.local/bin:\$PATH
                    
                    # Ensure Poetry is available globally
                    sudo ln -s \$HOME/.local/bin/poetry /usr/local/bin/poetry
                    
                    # Debug output to verify path and Poetry installation
                    echo \$PATH
                    poetry --version
                    "
                    '''
                }
            }
        }
        stage('Run Tests') {
            steps {
                script {
                    sh '''
                    # Run tests using Poetry
                    /bin/bash -c "poetry run pytest"
                    '''
                }
            }
        }
        stage('Deploy to Google App Engine') {
            steps {
                script {
                    sh '''
                    # Deploy to App Engine using Poetry
                    /bin/bash -c "poetry run gcloud app deploy"
                    '''
                }
            }
        }
    }
    post {
        always {
            echo 'Cleaning up...'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
