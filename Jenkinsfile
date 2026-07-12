pipeline {
    agent { label "devops-esa1" }
    tools { nodejs "NodeJS-18.16.0" }

    stages {
        stage('Build') {
            steps {
                sh ''' npm install'''
            }
        }
        stage('Unit Testing') {
            steps {
                sh '''npm test'''
            }
        }
        stage('Code Review') {
            steps {
                sh '''sonar-scanner \
                      -Dsonar.projectKey=simple-apps \
                      -Dsonar.sources=. \
                      -Dsonar.host.url=http://172.23.11.117:9000 \
                      -Dsonar.token=sqp_4b0e0e6fe54bc52c34b9d4020ebd4d1f847283f7'''
            }
        }
        stage('Deploy compose') {
            steps {
                sh '''
                docker compose build
                docker compose up -d
                '''
            }
        }
        stage('Push Image and Clean Image') {
            steps {
                sh '''
                docker tag simple-apps-pipeline esanugraha/simple-apps-pipeline
                docker push esanugraha/simple-apps-pipeline
                docker image prune -a -f
                '''
            }
        }
    }
}
