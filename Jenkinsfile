pipeline {
    agent { 
        node {
            label 'docker-agent-python-venv'  // your agent label
        }
    }
    triggers {
        pollSCM('* * * * *')
    }
    environment {
        VENV_PATH = "/home/jenkins/venv"
    }
    stages {

        stage('Build') {
            steps {
                echo "Building Python dependencies..."
                sh '''
                cd myapp
                source $VENV_PATH/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo "Running Python scripts..."
                sh '''
                cd myapp
                source $VENV_PATH/bin/activate
                python3 hello.py
                python3 hello.py --name=Devo
                '''
            }
        }

        stage('Deliver') {
            steps {
                echo 'Delivering...'
                sh '''
                echo "Doing delivery steps..."
                '''
            }
        }

    }
}
