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
    }
}