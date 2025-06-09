pipeline {
    agent any

    stages {
        stage ("Clone the code") {
            steps {
                git url: 'https://github.com/rohit5683/python-flask-app.git', branch: 'Master'
            }
        }

        stage ("Installing the dependencies") {
            steps {
                sh 'pip install -r requirement.txt'
            }
        }

        stage ("Testing the code") {
            steps {
                sh 'python -m pytest'
            }
        }

        stage ("Running Python App") {
            steps {
                sh 'python app.py'
            }
        }

    }
        
    }
}