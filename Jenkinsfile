pipeline {
  agent any
  stages {
    stage('hello world') {
      parallel {
        stage('hello world') {
          steps {
            sh 'echo "hello world"'
          }
        }

        stage('') {
          agent {
            docker {
              image 'gradle:6-jdk11'
            }

          }
          steps {
            sh 'ci/build-app.sh'
          }
        }

      }
    }

  }
}