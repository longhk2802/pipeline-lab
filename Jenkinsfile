pipeline {
  agent none
  stages {
    stage('Fluffy Build') {
      agent {
        node {
          label 'java8'
        }

      }
      steps {
        sh './jenkins/build.sh'
        archiveArtifacts 'target/*.jar'
        stash(name: 'Java 8', includes: 'target/**')
      }
    }

    stage('Fluffy Test') {
      parallel {
        stage('backend') {
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

        stage('fontend') {
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

        stage('statictics') {
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

        stage('performance') {
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
        sh './jenkins/deploy.sh staging'
      }
    }

  }
}