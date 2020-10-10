pipeline {
    agent any
    environment {
        COMMIT_ID="""${sh(returnStdout: true, script: 'git rev-parse --short HEAD')}"""
        app = ''
    }
    stages {
        stage('Build') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', '${DOCKER_CREDS}') {
                        app = docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                    }
                }
            }
        }
        stage('Test-ss') {
            steps {
                sh 'docker run -d -p 80:80 -p 9081:8080 -p 3000:80 -v datadoc:/data -e NAME=SERG -e AGE=48 ${IMAGE_NAME}:${IMAGE_TAG}'
            }
        }        
        stage('Push') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', '${DOCKER_CREDS}') {
                        app.push("${env.BUILD_ID}-${COMMIT_ID}")                
                    }
                }
            }
        }
    }

}
