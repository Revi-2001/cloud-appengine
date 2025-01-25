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
                    
                    # Install Poetry
                    curl -sS https://install.python-poetry.org | python3 -
                    
                    # Add Poetry to PATH for current session
                    export PATH="$HOME/.local/bin:$PATH"
                    "
                    '''
                }
            }
        }
        stage('Run Tests') {
            steps {
                script {
                    sh '''
                    # Run your tests using Poetry
                    poetry run pytest
                    '''
                }
            }
        }
        stage('Deploy to Google App Engine') {
            steps {
                script {
                    sh '''
                    # Deploy to App Engine using Poetry
                    poetry run gcloud app deploy
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
