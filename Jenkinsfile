pipeline {

    agent {
        docker {
            image 'bryandollery/alpine-docker'
            args "-u root --entrypoint=''"
        }
    }
    stages {
        stage ('generate manifest') {
            steps {
                sh """
cat <<EOF > ./manifest.txt
name: ${JOB_NAME}
time: ${currentBuild.startTimeInMillis}
build #: ${BUILD_NUMBER}
commit: ${GIT_COMMIT}
url: ${GIT_URL}
EOF
"""
            }
        }
        stage ('build') {
            steps {
                sh "docker build --tag manifest-holder:latest ."
                sh "docker tag manifest-holder manifest-holder:${BUILD_NUMBER}"
                sh "docker tag manifest-holder danya-mudaifea/manifest-holder:latest"
                sh "docker tag manifest-holder danya-mudaifea/manifest-holder:${BUILD_NUMBER}"
            }
        }
        stage ('test') {
            steps {
                sh "docker run --rm manifest-holder"
            }
        }
        stage ('release') {
            environment {
                CREDS = credentials('aws_danya')
            }
            steps {
                sh "whoami"
                sh "docker login -u ${CREDS_USR} -p ${CREDS_PSW}"
                sh "docker push danya-mudaifea/manifest-holder:${BUILD_NUMBER}"
                sh "docker push danya-mudaifea/manifest-holder:latest"
            }
        }
    }
} 
