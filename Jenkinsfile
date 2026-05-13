pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // This pulls the code from your GitHub repo
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Installing dependencies...'
                // We use --user to avoid permission issues in some environments
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // This runs the python tests included in the repo
                sh 'python3 -m pytest'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Application...'
                // This runs the app in the background so the pipeline can finish
                // JENKINS_NODE_COOKIE=dontKillMe prevents Jenkins from killing the app process
                sh 'JENKINS_NODE_COOKIE=dontKillMe nohup python3 app.py > app.log 2>&1 &'
            }
        }
    }
}
