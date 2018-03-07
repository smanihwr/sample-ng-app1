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
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh './node_modules/@angular/cli/bin/ng build --prod --aot --output-hashing none'

                sh '****************************************************************************'
                sh '****************************** Generated Files *****************************'
                sh '****************************************************************************'

                sh 'ls ./dist -ltr'
            }
        }

        stage('Test') {
            steps {
                sh 'node --version'
            }
        }
    }
}
