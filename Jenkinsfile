pipeline {

    stages {
        stage ("Clean Workspace") {
            agent { label 'test-server' }
            steps {
                deleteDir()
            }
        }
        
        stage ("Clone the code") {
            agent { label 'test-server' }
            steps {
                git url: 'https://github.com/rohit5683/python-flask-app.git', branch: 'master'
            }
        }

        stage ("Installing the dependencies") {
            agent { label 'test-server' }
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage ("Testing the code") {
            agent { label 'test-server' }
            steps {
                sh 'python -m pytest'
            }
        }

        stage ("Deploying Python App to Production Server") {
            agent { label 'prod-server' }
            steps {
                sh 'python app.py'
            }
        }

    }
        
}
