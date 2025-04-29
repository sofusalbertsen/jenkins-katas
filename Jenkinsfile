pipeline {
  agent any
  stages {
    stage('clone down'){
      steps{
        stash excludes: '.git', name: 'code'
      }
    }
    stage('hello world') {
      parallel {
        stage('hello world') {
          steps {
            sh 'echo "hello world"'
          }
        }

        stage('build app') {
          agent {
            docker {
              image 'gradle:6-jdk11'
            }

          }
          steps {
            sh 'ci/build-app.sh'
            archiveArtifacts 'app/build/libs/'
          }
        }

      }
    }

  }
}