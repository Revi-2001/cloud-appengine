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
                    # Update package manager and install pip if not present
                    sudo apt update
                    sudo apt install -y python3-pip python3-venv

                    # Create and activate a virtual environment
                    python3 -m venv venv
                    source venv/bin/activate

                    # Upgrade pip in the virtual environment
                    pip install --upgrade pip

                    # Install Poetry in the virtual environment
                    pip install poetry

                    # Ensure Poetry is available in the virtual environment
                    source venv/bin/activate
                    poetry --version
                    '''
                }
            }
        }
        stage('Run Tests') {
            steps {
                script {
                    sh '''
                    # Activate the virtual environment
                    source venv/bin/activate

                    # Install project dependencies using Poetry
                    poetry install

                    # Run tests
                    poetry run pytest
                    '''
                }
            }
        }
        stage('Deploy to Google App Engine') {
            steps {
                script {
                    sh '''
                    # Activate the virtual environment
                    source venv/bin/activate

                    # Deploy to Google App Engine
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
