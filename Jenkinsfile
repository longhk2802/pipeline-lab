pipeline {
  agent none
  stages {
    stage('Fluffy Build') {
      parallel {
        stage('build java 7') {
          agent {
            node {
              label 'java7'
            }

          }
          post {
            success {
              archiveArtifacts 'target/*.jar'
              stash(name: 'Java 7', includes: 'target/**')
            }

          }
          steps {
            sh './jenkins/build.sh'
          }
        }

        stage('build java 8') {
          agent {
            node {
              label 'java8'
            }

          }
          post {
            success {
              stash(name: 'Java 8', includes: 'target/**')
            }

          }
          steps {
            sh './jenkins/build.sh'
          }
        }

      }
    }

    stage('Fluffy Test') {
      parallel {
        stage('backend java 7') {
          agent {
            node {
              label 'java7'
            }

          }
          post {
            always {
              junit(testResults: 'target/surefire-reports/**/TEST*.xml', skipPublishingChecks: true)
            }

          }
          steps {
            unstash 'Java 7'
            sh './jenkins/test-backend.sh'
          }
        }

        stage('fontend java 7') {
          agent {
            node {
              label 'java7'
            }

          }
          post {
            always {
              junit 'target/test-results/**/TEST*.xml'
            }

          }
          steps {
            unstash 'Java 7'
            sh './jenkins/test-frontend.sh'
          }
        }

        stage('statictics java 7') {
          agent {
            node {
              label 'java7'
            }

          }
          steps {
            unstash 'Java 7'
            sh './jenkins/test-static.sh'
          }
        }

        stage('performance java 7') {
          agent {
            node {
              label 'java7'
            }

          }
          steps {
            unstash 'Java 7'
            sh './jenkins/test-performance.sh'
          }
        }

        stage('backend java 8') {
          agent {
            node {
              label 'java8'
            }

          }
          post {
            always {
              junit(testResults: 'target/surefire-reports/**/TEST*.xml', skipPublishingChecks: true)
            }

          }
          steps {
            unstash 'Java 8'
            sh './jenkins/test-backend.sh'
          }
        }

        stage('fontend java 8') {
          agent {
            node {
              label 'java8'
            }

          }
          post {
            always {
              junit 'target/test-results/**/TEST*.xml'
            }

          }
          steps {
            unstash 'Java 8'
            sh './jenkins/test-frontend.sh'
          }
        }

        stage('statictics java 8') {
          agent {
            node {
              label 'java8'
            }

          }
          steps {
            unstash 'Java 8'
            sh './jenkins/test-static.sh'
          }
        }

        stage('performance java 8') {
          agent {
            node {
              label 'java8'
            }

          }
          steps {
            unstash 'Java 8'
            sh './jenkins/test-performance.sh'
          }
        }

      }
    }

    stage('Confirm Deploy') {
      when {
        branch 'master'
      }
      steps {
        input(message: 'Okay to deploy to staging?', ok: 'yes')
      }
    }

    stage('Fluffy Deploy') {
      agent {
        node {
          label 'java8'
        }

      }
      when {
        branch 'master'
      }
      steps {
        unstash 'Java 7'
        sh './jenkins/deploy.sh staging'
      }
    }

  }
}