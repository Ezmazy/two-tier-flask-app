pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // This downloads your forked repo
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Creating Virtual Environment and Installing Dependencies...'
                // 1. Create the 'venv' folder
                // 2. Use the pip inside that folder to install requirements
                sh '''
                python3 -m venv venv
                ./venv/bin/pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests within the Virtual Environment...'
                // Use the python inside the venv to run pytest
                sh './venv/bin/python3 -m pytest'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Application...'
                // 1. Kill any app already on port 5000
                // 2. Run the app using the venv python
                sh '''
                fuser -k 5000/tcp || true
                JENKINS_NODE_COOKIE=dontKillMe nohup ./venv/bin/python3 app.py > app.log 2>&1 &
                '''
            }
        }
    }
}
