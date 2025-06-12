pipeline {
    agent any

    environment {
        SERVER_IP = credentials('production-server-ip')
    }

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
        
        stage ("Packaging the file") {
            steps {
                sh 'zip -r myapp.zip . -x '*.git*' '*.gitignore*' '
            }
        }

        stage ("Deploying Python App to Production Server") {
            stages {
                withCredentials([sshUserPrivateKey(credentialsId: 'ssh-key', keyFileVariable: 'MY_SSHKEY', usernameVariable: 'username')]) {
                sh '''scp -i ${MY_SSHKEY} myapp.zip ${username}@${SERVER_IP}:/home/ec2-user/'
                unzip myapp.zip && cd myapp
                pip install -r requirements.txt
                '''
    }
            }
            
        }

    }
        
}
