pipeline {
    agent any
    stages {
        stage('1. Compile & Test') {
            steps {
                // CI: Verification stage
                sh './mvnw clean verify'
            }
        }
        stage('2. Package with Jib') {
            steps {
                // CD: Containerization without a Dockerfile
                sh './mvnw compile jib:dockerBuild'
            }
        }
        stage('3. Deploy Locally') {
            steps {
                // CD: Orchestration
                sh 'docker rm -f spring-demo || true'
                sh 'docker run -d --name spring-demo -p 8081:8080 my-spring-app:latest'
            }
        }
    }
}