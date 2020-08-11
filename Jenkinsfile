pipeline {

    agent {
        docker {
            image 'bryandollery/alpine-docker'
            args "-u root --entrypoint=''"
        }
    }
    environment {
      CREDS = credentials('aws-creds')
      AWS_ACCESS_KEY_ID = "$CREDS_USR"
      AWS_SECRET_ACCESS_KEY = "$CREDS_PSW"
      OWNER = 'bryan'
      PROJECT_NAME = 'web-server'
    }
  stages {
    stage("build") {
      steps {
        sh 'packer build --tag packer packer.json'
              }
        }
       stage ('test') {
            steps {
                sh "docker run --rm manifest-holder"
            }
        }

    }
} 
