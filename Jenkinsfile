pipeline {
    agent any

    environment {
        strDockerImage = 'minkyuu98/cicd-test:0.1'
    }
    stages {
        stage('Github Pull') {
            steps {
                git branch: 'main', url:'https://github.com/minkyuu98/cicd-repo.git'
            }
        }
                stage('Docker Image Build') {
            steps {
                script {
                    oDckerImage = docker.build(strDockerImage, '-f Dockerfile .')
                }
            }
                }

        stage('Docker Image Push') {
            steps {
                script {
                    docker.withRegistry('', 'docker-auth') {
                        oDckerImage.push()
                    }
                }
            }
        }
        stage('Deploy Server') {
            steps {
                sshagent(credentials: ['Deploy-Privatekey']) {
                    sh 'scp -o StrictHostKeyChecking=no index.html ubuntu@3.35.235.103:/home/ubuntu/'
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@3.35.235.103 sudo cp /home/ubuntu/index.html /var/www/html'
                }
            }
        }
            }
        }
