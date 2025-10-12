pipeline {
    agent {label 'devops1-esa'}
    tools {nodejs 'NodeJS 18.16.0'}

    stages {
        stage ('Git Clone SCM'){
            steps {
                git branch: 'main', url: 'https://github.com/annesa123/simple-apps.git'
            }
        }
        stage ('Unit Testing') {
            steps {
                sh '''
                npm install
                npm test'''
            }
        }
        stage('Code Review') {
            steps {
                sh '''
                sonar-scanner \
                -Dsonar.projectKey=simple-apps \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://172.23.7.31:9000 \
                -Dsonar.login=sqp_afa8fce16ce7c05e3ae2a24d10ea33a8ce8a9807'''
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
        stage('Push Image to DockerHub') {
            steps {
                sh '''
                docker build -t esanugraha/simple-apps-pipeline-apps .
                docker push esanugraha/simple-apps-pipeline-apps
                docker images prune -a -f
                '''
            }
        }
    }
}