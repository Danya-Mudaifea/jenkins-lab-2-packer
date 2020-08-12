 
pipeline {
  agent {
    docker {
      image "bryandollery/terraform-packer-aws-alpine"
      args "-u root --entrypoint='' --rm"
    }
  }
  environment {
    CREDS = credentials('amerah-creds')
    AWS_ACCESS_KEY_ID = "${CREDS_USR}"
    AWS_SECRET_ACCESS_KEY = "${CREDS_PSW}"
    OWNER = 'danya'
    PROJECT_NAME = 'web-server'
  }
  stages {
    stage("build") {
      steps {
        sh 'packer build packer.json'
      }
    }
  }
  post {
    success {
        build quietPeriod: 0, wait: false, job: 'jenkins-lab-2-tf'  
    }
  }
}
