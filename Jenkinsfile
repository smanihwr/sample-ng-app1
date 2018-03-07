pipeline {
    agent {
        docker { image 'node:7-alpine' }
    }

    environment {
        npm_config_cache= 'npm-cache'
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
                sh 'pwd'
                sh 'find . -name ng'
                sh './node_modules/@angular/cli/bin/ng build --prod --aot --output-hashing none'
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
