stage('Install Dependencies') {
    steps {
        script {
            sh '''
            sudo apt update
            sudo apt install -y python3-pip python3-venv
            python3 -m venv venv
            source venv/bin/activate
            pip install --upgrade pip
            pip install poetry
            '''
        }
    }
}
