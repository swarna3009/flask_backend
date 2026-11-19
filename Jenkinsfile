
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Flask source code checked out by Jenkins'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    python3 -m venv venv
                    venv/bin/pip install -r requirements.txt
                '''
            }
        }

        stage('Deploy Flask') {
            steps {
                echo 'Flask deployment stage'
            }
        }
    }
}
