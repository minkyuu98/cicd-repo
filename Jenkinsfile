import java.text.SimpleDateFormat

def TODAY = (new SimpleDateFormat('yyyyMMdd')).format(new Date())

pipeline {
    agent any

    environment {
        strDockertag =  "${TODAY}_${BUILD_ID}"
        strDockerImage = "minkyuu98/cicd-test:${strDockertag}"
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
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@3.35.235.103 docker container rm -f sampleweb'
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@3.35.235.103 docker run -d -p 80:80 --name sampleweb ${strDockerImage}'
                }
            }
        }
    }
}
