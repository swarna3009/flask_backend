
pipeline {
    agent any

    options {
        timestamps()
        timeout(time: 15, unit: 'MINUTES')
    }

    environment {
        APP_DIR = '/home/ubuntu/projects/flask_backend'
        VENV = '/home/ubuntu/projects/venv'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Validate Project') {
            steps {
                sh '''
                    test -f app.py
                    test -f requirements.txt
                    echo "Flask project files found"
                '''
            }
        }

        stage('Deploy Latest Code') {
            steps {
                sh '''
                    rsync -rltD --no-owner --no-group --no-perms --delete \
                      --exclude='.git/' \
                      --exclude='__pycache__/' \
                      --exclude='*.pyc' \
                      "$WORKSPACE/" "$APP_DIR/"
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    "$VENV/bin/pip" install -r "$APP_DIR/requirements.txt"
                '''
            }
        }

        stage('Restart Flask') {
            steps {
                sh '''
                    sudo -n -u ubuntu /usr/local/bin/pm2 restart flask-backend
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                    curl --fail --silent --show-error \
                      --retry 5 --retry-delay 2 \
                      http://127.0.0.1:5000/api
                '''
            }
        }
    }

    post {
        success {
            echo 'Flask backend deployed successfully!'
        }

        failure {
            echo 'Flask deployment failed. Check the console output.'
        }
    }
}
