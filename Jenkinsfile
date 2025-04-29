pipeline {
  agent any
  environment {
    docker_username = 'praqmasofus'
  }
  stages {
    stage('Clone down'){
      agent any
      steps{
        stash excludes: '.git', name: 'code'
      }
    }
    stage('Parallel execution') {
      parallel {
        stage('Say Hello') {
          steps {
            sh 'echo "hello world"'
          }
        }

        stage('build app') {
          options {
            skipDefaultCheckout()
          }
          agent {
            docker {
              image 'gradle:6-jdk11'
            }

          }
          steps {
            unstash 'code'
            sh 'ci/build-app.sh'
            stash excludes: '.git', name: 'code'
            archiveArtifacts 'app/build/libs/'
          }
        }
        stage('test app') {
          options {
            skipDefaultCheckout()
          }
          agent {
            docker {
              image 'gradle:6-jdk11'
            }

          }
          steps {
            unstash 'code'
            sh 'ci/unit-test-app.sh'
            
            junit 'app/build/test-results/test/TEST-*.xml'
          }
        }

      }
        }
        stage('push app') {
          options {
            skipDefaultCheckout()
          }
          agent any

          environment {
      DOCKERCREDS = credentials('docker_login') //use the credentials just created in this stage
}
steps {
      unstash 'code' //unstash the repository code
      sh 'ci/build-docker.sh'
      sh 'echo "$DOCKERCREDS_PSW" | docker login -u "$DOCKERCREDS_USR" --password-stdin' //login to docker hub with the credentials above
      sh 'ci/push-docker.sh'
}
    }

  }
}
