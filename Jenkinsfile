pipeline {
  agent {
    node {
      label 'java7'
    }

  }
  stages {
    stage('Fluffy Build') {
      steps {
        sh './jenkins/build.sh'
        archiveArtifacts 'target/*.jar'
      }
    }

    stage('Fluffy Test') {
      parallel {
        stage('backend') {
          steps {
            sh './jenkins/test-backend.sh'
            junit(testResults: 'target/surefire-reports/**/TEST*.xml', skipPublishingChecks: true)
          }
        }

        stage('fontend') {
          steps {
            sh './jenkins/test-frontend.sh'
            junit 'target/test-results/**/TEST*.xml'
          }
        }

        stage('statictics') {
          steps {
            sh './jenkins/test-static.sh'
          }
        }

        stage('performance') {
          steps {
            sh './jenkins/test-performance.sh'
          }
        }

      }
    }

    stage('Fluffy Deploy') {
      steps {
        sh './jenkins/deploy.sh staging'
      }
    }

  }
}