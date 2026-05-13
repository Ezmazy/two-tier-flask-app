pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Downloads your code from GitHub
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Installing dependencies...'
                // --break-system-packages is sometimes needed in newer Ubuntu versions
                // We use python3 -m pip to ensure we use the correct version
                sh 'python3 -m pip install --user -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // This command tells Python to run the pytest module on your code
                sh 'python3 -m pytest'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Application...'
                // 1. Kill any app already running on port 5000 so the new one can start
                // 2. Start the app in the background
                sh '''
                fuser -k 5000/tcp || true
                JENKINS_NODE_COOKIE=dontKillMe nohup python3 app.py > app.log 2>&1 &
                '''
            }
        }
    }
}
