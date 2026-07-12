pipeline {
    agent { label "devops-esa1" }
    tools { nodejs "NodeJS-18.16.0" }
    
environment {
   NAMEAPPS = 'simple-apps-pipeline-apps'
   SONARHOST = 'http://172.23.11.117:9000'
   TOKENSONAR = 'sqp_4b0e0e6fe54bc52c34b9d4020ebd4d1f847283f7'
   VERSION = 'v1'
}

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
                      -Dsonar.host.url=${SONARHOST} \
                      -Dsonar.token=${TOKENSONAR}'''
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
                docker tag ${NAMEAPPS} esanugraha/${NAMEAPPS}:${VERSION}
                docker push esanugraha/${NAMEAPPS}:${VERSION}
                docker image prune -a -f
                '''
            }
        }
    }
}
