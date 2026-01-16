pipeline {
    agent { 
        node {
            label 'docker-agent-python'  // your agent label
        }
    }
    triggers {
        pollSCM '* * * * *'
    }
    environment {
        # Path to the Python virtual environment inside your Docker agent
        VENV_PATH = "/home/jenkins/venv"
    }
    stages {

        stage('Build') {
            steps {
                echo "Building Python dependencies..."
                sh '''
                cd myapp
                # Activate virtual environment
                source $VENV_PATH/bin/activate
                # Install dependencies
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
                # Activate virtual environment
                source $VENV_PATH/bin/activate
                # Run scripts
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
