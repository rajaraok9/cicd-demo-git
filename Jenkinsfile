pipeline {
    agent any
    tools {
            maven 'Maven_3.8.5'
        }
        environment {
                APP_NAME   = "spring-demo"
                REGISTRY   = "docker.io/rajadocker2109"
                IMAGE_NAME = "${REGISTRY}/${APP_NAME}"
                IMAGE_TAG  = "latest"
            }
    stages {

           stage('1. CI - Checkout') {
                    steps {
                        git branch: 'main',
                            credentialsId: 'git-creds',
                            url: 'https://github.com/rajaraok9/cicd-demo-git.git'
                    }
                }
                stage('2. Compile & Test') {
                            steps {
                                // CI: Verification stage
                                sh './mvnw clean compile'
                            }
                        }
                stage('CI - Unit Tests') {
                            steps {
                                sh 'mvn test'
                            }
                        }
        stage('CI - Build Image (Jib)') {
                    steps {
                        withCredentials([usernamePassword(
                            credentialsId: 'dockerhub-creds',
                            usernameVariable: 'DOCKER_USER',
                            passwordVariable: 'DOCKER_PASS'
                        )]) {
                            sh """
                              mvn jib:build \
                                -Djib.to.image=${IMAGE_NAME} \
                                -Djib.to.tags=${IMAGE_TAG} \
                                -Djib.to.auth.username=${DOCKER_USER} \
                                -Djib.to.auth.password=${DOCKER_PASS}
                            """
                        }
                    }
                }
        stage('3. Deploy Locally') {
            steps {
                // CD: Orchestration will be done here
                sh 'docker rm -f spring-demo || true'
                sh 'docker run -d --name spring-demo -p 8081:8080 my-spring-app:latest'
            }
        }
    }
}