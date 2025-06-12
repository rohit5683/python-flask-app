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
                withCredentials([
                    string(credentialsId: 'git_url', variable: 'GIT_REPO_URL'),
                    string(credentialsId: 'git_branch', variable: 'GIT_REPO_BRANCH')
                ]) {
                    git branch: "${GIT_REPO_BRANCH}",
                        url: "${GIT_REPO_URL}"
                }
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
                    sh '''
                    scp -i $MY_SSHKEY -o StrictHostKeyChecking=no myapp.zip  ${username}@${SERVER_IP}:/home/ec2-user/
                    ssh -i $MY_SSHKEY -o StrictHostKeyChecking=no ${username}@${SERVER_IP} << EOF
                        unzip -o /home/ec2-user/myapp.zip -d /home/ec2-user/app/
                        cd /home/ec2-user/app/
                        python3 -m venv venv
                        source venv/bin/activate
                        pip install -r requirements.txt
                        sudo systemctl restart flaskapp.service
EOF
                    '''
                }
            }
        }
    }
}
