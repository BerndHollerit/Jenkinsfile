pipeline {
  agent {
    label 'master'
  }
  stages {
    stage('Build') {
      parallel {
        stage('App build') {
          steps {
            sh './gradlew :app:build'
          }
        }

        stage('App compileJsp') {
          steps {
            sh './gradlew :app:compile'
          }
        }

      }
    }

    stage('Test') {
      steps {
        sh './gradlew :app:test'
      }
    }

    stage('Jacoco Test') {
      steps {
        sh './gradlew :app:testCoverageVerification'
      }
    }

    stage('StaticAnalysis') {
      parallel {
        stage('StaticAnalysis') {
          steps {
            sh './gradlew :app:checkstyleMain'
          }
        }

        stage('pmdMain') {
          steps {
            sh './gradlew :app:pmdMain'
          }
        }

        stage('spotBugsMain') {
          steps {
            sh './gradlew :app:spotBugsMain'
          }
        }

      }
    }

//     stage('Deploy') {
//       steps {
//         sh './gradlew :app:deploy'
//       }
//     }

    stage('Report') {
      steps {
        office365ConnectorSend(webhookUrl: 'https://outlook.office.com/webhook/dummy/url', color: 'Green', message: 'Jenkins pipeline notification - Bernd Hollerit', status: 'Jenkins report sent')
      }
    }

  }
  post {
    always {
      echo 'Pipeline finished'
    }

    success {
      echo 'Pipeline succeeded!'
    }

    unstable {
      echo 'Pipeline is unstable'
    }

    failure {
      echo 'Pipeline failed'
    }

    changed {
      echo 'Pipeline changed'
    }

  }
  triggers {
    cron('H H * * *')
  }
}