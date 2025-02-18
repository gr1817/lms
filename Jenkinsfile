pipeline {
    agent any
    stages {
        stage('Code quality') {
            steps {
                sh 'cd webapp && sudo docker run --rm -e SONAR_HOST_URL="http://34.220.107.216:9000/" -v ".:/usr/src" -e SONAR_TOKEN="sqa_d6d5b683013b8c5c009a3076122fdecab8d9f52c" sonarsource/sonar-scanner-cli -Dsonar.projectKey=lms'
            }
        }
        stage('Build') {
            steps {
                sh 'cd webapp && npm install && npm run build'
            }
        }
        stage('Clean Up') {
            steps {
                cleanWs()
            }
        }
    }
}