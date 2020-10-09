pipeline {
    agent any
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
            agent docker
            steps {
                bash -c "docker run -t ${IMAGE_NAME}:${IMAGE_TAG} "
            }
        }
        
         stage("Run tests") {
            steps {
                bash -c "docker exec -t ${IMAGE_NAME}:${IMAGE_TAG} /bin/bash"
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
