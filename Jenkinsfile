pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Setting up the Python virtual environment...'
                sh '''
                python3 -m venv venv
                ./venv/bin/pip install --upgrade pip
                ./venv/bin/pip install -r requirements.txt
                ./venv/bin/pip install pytest flask
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // This looks for any file starting with test_
                sh './venv/bin/python3 -m pytest'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Launching Application...'
                sh '''
                # This clears port 5000 so the app can start fresh
                fuser -k 5000/tcp || true
                
                # Starts the app in the background using the venv
                JENKINS_NODE_COOKIE=dontKillMe nohup ./venv/bin/python3 app.py > app.log 2>&1 &
                '''
            }
        }
    }
}
