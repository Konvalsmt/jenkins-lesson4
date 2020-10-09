pipeline {
    agent Docker
    environment {
       
        app = ''
    }
    stages {
        stage('Build') {
            steps {
                script {
                    app = docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }
        
        stage('Test') {
            steps {
                sh 'docker run -it ${IMAGE_NAME}:${IMAGE_TAG} '
            }
        }
        
        stage('Push') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', '${DOCKER_CREDS}') {
                        app.push("${env.BUILD_ID} ")                
                    }
                }
            }
        }
    }

}