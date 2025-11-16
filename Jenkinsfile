pipeline {
    agent { label "dev01-esa" }
    tools { nodejs "NodeJS-18.16.0" }

    stages {
        stage('Build') {
            steps {
                sh ''' npm install'''
            }
        }
        stage('Unit Testing') {
            steps {
                sh '''npm test
                npm run test:coverage'''
            }
        }
        stage('Code Review') {
            steps {
                sh '''sonar-scanner \
                -Dsonar.projectKey=simple-apps \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://172.23.5.4:9000 \
                -Dsonar.login=sqp_d54d727c34b1893ad1bcd1167e76b43e6dbb8b8a'''
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
                docker tag simple-apps esanugraha/simple-apps
                docker push esanugraha/simple-apps
                docker prune -a -f
                '''
            }
        }
    }
}