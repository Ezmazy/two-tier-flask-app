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
                echo 'Installing dependencies...'
                // Using 'python3 -m pip' is more reliable than just 'pip'
                sh 'python3 -m pip install --user -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Using 'python3 -m pytest' ensures it uses the correct python version
                sh 'python3 -m pytest'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Application...'
                // This starts the app in the background
                sh 'JENKINS_NODE_COOKIE=dontKillMe nohup python3 app.py > app.log 2>&1 &'
            }
        }
    }
}
