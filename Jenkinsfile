pipeline {
    agent {label 'devops-1-esa'}
    tools {nodejs 'nodejs-18.16.0'}

    stages {
        stage('checkout GIT') {
            steps {
                git branch: 'main', url: 'https://github.com/annesa123/simple-apps.git'
            }
        }

        stage('Run NPM') {
            steps {
                sh '''npm install
                '''
            }
        }

        stage('SonarQube') {
            steps {
                sh '''sonar-scanner \
                -Dsonar.projectKey=simple-apps \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://172.23.1.11:9000  \
                -Dsonar.login=sqp_c628928a800843c1972f680103dd35e2d09c64cc
                '''
            }
        }

        stage('Docker Compose') {
            steps {
                sh '''docker compose build
                docker compose up -d
                '''
            }
        }
    }   
}
