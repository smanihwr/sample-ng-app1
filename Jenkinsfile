pipeline {
    agent {
        docker { image 'node:7-alpine' }
    }
    stages {

        stage('Setup ng environment') {
            steps {
                sh 'pwd'
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'ng build --prod --aot --output-hashing none'
                sh 'pwd'
                sh 'cd dist'
                sh 'ls -ltr'
            }
        }

        stage('Test') {
            steps {
                sh 'node --version'
            }
        }
    }
}
