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
        stage('Publish LMS') {
            steps {
                script {
                    def packageJson = readJSON file: 'webapp/package.json'
                    def packageJSONVersion = packageJson.version
                    echo "${packageJSONVersion}"
                    sh "zip webapp/lms-${packageJSONVersion}.zip -r webapp/dist"
                    // Try with POST or -F for file upload
                    sh "curl -v -u admin:Gopirajuyadala@78 -F \"file=@webapp/lms-${packageJSONVersion}.zip\" http://34.220.107.216:8081/repository/lms/"
                }
            }
        }
        stage('Deploy LMS') {
            steps {
                script {
                    def packageJson = readJSON file: 'webapp/package.json'
                    def packageJSONVersion = packageJson.version
                    echo "${packageJSONVersion}"
                    // Download and check if the file is a valid zip file
                    sh "curl -u admin:Gopirajuyadala@78 -X GET 'http://34.220.107.216:8081/repository/lms/lms-${packageJSONVersion}.zip' --output lms-${packageJSONVersion}.zip"
                    // Verify file before unzipping
                    sh "if [ -f lms-${packageJSONVersion}.zip ]; then unzip -o lms-${packageJSONVersion}.zip; else echo 'File is missing or not a valid zip'; fi"
                    sh "sudo rm -rf /var/www/html/*"
                    sh "sudo cp -r webapp/dist/* /var/www/html"
                }
            }
        }

        stage('Clean Up') {
            steps {
                cleanWs()
            }
        }
    }
}
