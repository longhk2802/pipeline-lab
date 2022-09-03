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
          steps {
            sh './jenkins/build.sh'
            archiveArtifacts 'target/*.jar'
            stash(name: 'Java 7', includes: 'target/**')
          }
        }

        stage('build java 8') {
          agent {
            node {
              label 'java8'
            }

          }
          steps {
            sh './jenkins/build.sh'
            stash(name: 'Java 8', includes: 'target/**')
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
          steps {
            unstash 'Java 7'
            sh './jenkins/test-backend.sh'
            junit(testResults: 'target/surefire-reports/**/TEST*.xml', skipPublishingChecks: true)
          }
        }

        stage('fontend java 7') {
          agent {
            node {
              label 'java7'
            }

          }
          steps {
            unstash 'Java 7'
            sh './jenkins/test-frontend.sh'
            junit 'target/test-results/**/TEST*.xml'
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
          steps {
            unstash 'Java 8'
            sh './jenkins/test-backend.sh'
            junit(testResults: 'target/surefire-reports/**/TEST*.xml', skipPublishingChecks: true)
          }
        }

        stage('fontend java 8') {
          agent {
            node {
              label 'java8'
            }

          }
          steps {
            unstash 'Java 8'
            sh './jenkins/test-frontend.sh'
            junit 'target/test-results/**/TEST*.xml'
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

    stage('Fluffy Deploy') {
      agent {
        node {
          label 'java8'
        }

      }
      steps {
        unstash 'Java 7'
        sh './jenkins/deploy.sh staging'
      }
    }

  }
}