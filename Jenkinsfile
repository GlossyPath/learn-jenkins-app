  pipeline {
      agent any

      stages {
          stage('Build') {
              agent {
                  docker {
                      image 'node:18-alpine'
                      reuseNode true
                  }
              }
              steps {
                  sh '''
                      ls -la
                      node --version
                      npm --version
                      npm ci
                      npm run build
                      ls -la
                  '''
              }
          }

          stage('Test') {
           agent {
            docker {
              image 'node:18-alpine'
              reuseNode true
                    }
                  }
              steps {
              echo 'Testing starting'
              sh '''
                test -f build/index.html   //looking for a file with test -f
                npm test
              '''
              }
          }
      }
  }