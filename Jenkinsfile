pipeline {
    agent any

    stages {
        stage ("Clean Workspace") {
            steps {
                deleteDir()
            }
        }
        stage ("Clone the code") {
            steps {
                git url: 'https://github.com/rohit5683/python-flask-app.git', branch: 'master'
            }
        }

        stage ("Installing the dependencies") {
            steps {
                sh 'pip install -r requirements.txt'
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