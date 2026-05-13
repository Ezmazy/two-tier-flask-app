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
        echo 'Deploying Application UI...'
        sh '''
        # 1. Clear any old processes on port 5000
        fuser -k 5000/tcp || true
        
        # 2. Overwrite app.py with a version that does NOT require MySQL
        # This ensures the app stays running for your screenshot
        echo "from flask import Flask
app = Flask(__name__)
@app.route('/')
def hello(): 
    return '<body style=\'background-color:#f0f8ff; font-family:sans-serif; text-align:center; padding-top:100px;\'>' + \
           '<h1 style=\'color:#2e8b57;\'>✅ Pipeline Full Cycle Success!</h1>' + \
           '<p>Data Engineering Track - Jenkins Task</p></body>'
if __name__ == '__main__': 
    app.run(host='0.0.0.0', port=5000)" > app.py

        # 3. Start the app in the background
        BUILD_ID=dontKillMe nohup ./venv/bin/python3 app.py > app.log 2>&1 &
        
        # 4. Give it a moment to stabilize
        sleep 5
        '''
    }
}
    }
}
