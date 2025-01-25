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
                    # Ensure pip is installed
                    python3 -m ensurepip --upgrade
                    python3 -m pip install --upgrade pip
                    
                    # Install Poetry
                    curl -sS https://install.python-poetry.org | python3 -
                    export PATH="/var/lib/jenkins/.local/bin:$PATH"
                    '''
                }
            }
        }
        stage('Run Tests') {
            steps {
                script {
                    sh '''
                    # Run your tests here
                    poetry run pytest
                    '''
                }
            }
        }
        stage('Deploy to Google App Engine') {
            steps {
                script {
                    sh '''
                    # Deploy to App Engine
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
