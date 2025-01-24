pipeline {
    agent any

    environment {
        PROJECT_ID = 'optimistic-yew-442501-g8'
        GOOGLE_APPLICATION_CREDENTIALS = credentials('app-service-account')  // Service account credential
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Revi-2001/cloud-appengine.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Install Python and pip locally (without sudo)
                    sh '''
                    curl -sS https://install.python-poetry.org | python3 -
                    export PATH="$HOME/.local/bin:$PATH"
                    pip install --upgrade pip
                    pip install -r requirements.txt
                    '''
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run your tests here (if any)
                    sh 'pytest'
                }
            }
        }

        stage('Deploy to Google App Engine') {
            steps {
                script {
                    // Authenticate with Google Cloud
                    sh 'gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}'

                    // Set project ID
                    sh 'gcloud config set project $PROJECT_ID'

                    // Deploy to App Engine
                    sh 'gcloud app browse'
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            // Optional: Clean up after the pipeline
        }

        success {
            echo 'Deployment successful!'
        }

        failure {
            echo 'Deployment failed!'
        }
    }
}
