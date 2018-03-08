pipeline {

    agent {
        docker { image 'ubuntu' }
    }

    environment {
        npm_config_cache= 'npm-cache'
    }

    stages {

        stage('Setup Node and Ng') {
            steps {
                sh 'apt-get install sudo -y'
                sh 'apt-get update'

                sh 'apt-get install wget -y'
                sh 'apt-get install apt-transport-https -y'

                sh 'apt-get install nodejs -y'
                sh 'apt-get install npm -y'
                sh 'npm install -y'
            }
        }

        stage('Setup cf-cli') {
            steps {
                // ...first add the Cloud Foundry Foundation public key and package repository to your system
                sh 'wget -q -O - https://packages.cloudfoundry.org/debian/cli.cloudfoundry.org.key | apt-key add -'
                sh 'echo "deb https://packages.cloudfoundry.org/debian stable main" | sudo tee /etc/apt/sources.list.d/cloudfoundry-cli.list'
                // ...then, update your local package index, then finally install the cf CLI
                sh 'apt-get update'
                sh 'apt-get install cf-cli'
            }
        }

        stage('Build') {
            steps {
                sh './node_modules/@angular/cli/bin/ng build --prod --aot --output-hashing none'

                println "****************************** Generated Files *****************************"
                sh 'ls ./dist -ltr'
                println "****************************************************************************"

            }
        }

        stage('Test') {
            steps {
                sh 'node --version'
            }
        }

        stage('Deploy to Cloud Foundry') {
            steps {
                sh 'cf login -a https://api.run.pivotal.io --skip-ssl-validation -u nabise@wmail.club -p Pa55word$ -o my-dev1-org'
                sh 'cf push -p ./dist'
            }
        }

    }
}
