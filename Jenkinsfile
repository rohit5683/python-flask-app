pipeline {
    agent any

    environment {
        SERVER_IP = credentials('production_server_ip')
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
                sh 'zip -r myapp.zip . -x "*.git*" "*.gitignore*"'
            }
        }

        stage ("Deploying Python App to Production Server") {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'ssh-key', keyFileVariable: 'MY_SSHKEY', usernameVariable: 'username')]) {
                    // Upload the zip file
                    sh 'scp -i $MY_SSHKEY -o StrictHostKeyChecking=no myapp.zip ${username}@${SERVER_IP}:/home/ec2-user/'

                    // Run remote commands via SSH
                    sh """
                        ssh -i ${MY_SSHKEY} -o StrictHostKeyChecking=no ec2-user@${SERVER_IP} << 'EOF'
                        cd /home/ec2-user/myapp
                        pip install -r requirements.txt --user
                        nohup python3 app.py > app.log 2>&1 &
                        EOF
                        """

                }
            }
        }
    }
}
