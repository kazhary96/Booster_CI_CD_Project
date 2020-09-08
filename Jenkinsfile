pipeline {
    agent {label 'slave'}
    stages {

        stage('preparation') {
            steps {
                git 'https://github.com/kazhary96/Booster_CI_CD_Project'
            }
        }

        stage('build') {
            steps {
              sh 'docker build -t kazhary96/django_app:v1.0 .'
            }
            }

        stage('push') {
            steps {
              withCredentials([usernamePassword(credentialsId:"docker",usernameVariable:"USERNAME",passwordVariable:"PASSWORD")]){
              sh 'docker login --username $USERNAME --password $PASSWORD'
              sh 'docker push kazhary96/django_app:v1.0'
              }
            }
        }

        stage('deploy') {
          steps {
            sh 'docker run -d -p 8000:8000 kazhary96/django_app:v1.0'
        }
        }
    }

    post {
      success {
      slackSend (color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME}  [${env.BUILD_NUMBER}]' (${env.BUILD_URL}console)")
      }

    }
}
