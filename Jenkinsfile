def withMyCredentials(body) {
  withCredentials([
    [ $class: 'StringBinding',
      credentialsId: 'PCF-DEV',
      passwordVariable: 'PASSWORD_VAR',
      usernameVariable: 'USERNAME_VAR']
  ],
 body)
}

pipeline {

    agent {
        docker { image 'node:7-alpine' }
    }

    environment {
        npm_config_cache= 'npm-cache'
    }

    stages {

        stage('Setup ng and cf-cli') {
            steps {
                sh 'npm install'

                // ...first add the Cloud Foundry Foundation public key and package repository to your system
                sh 'wget -q -O - https://packages.cloudfoundry.org/debian/cli.cloudfoundry.org.key | sudo apt-key add -'
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

        // stage('Deploy to Cloud Foundry') {
        //     withCredentials([usernamePassword(credentialsId: 'PCF_USER', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
        //         sh 'cf login -a https://api.run.pivotal.io --skip-ssl-validation -u USERNAME -p PASSWORD -o my-dev-org'
        //         sh 'cf push -p ./dist'
        //     }
        // }

        stage ("Global Credentials Overwritten at the user scope") {
            // credentials declared globally and overwritten by a user scoped credentials
            withMyCredentials {
               sh "echo $USERNAME_VAR"
               sh "echo $PASSWORD_VAR"
            }
        }
    }
}
