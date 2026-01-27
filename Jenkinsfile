pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    //guarda todo en el mismo workspace
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
            //test - f es para buscar un archivo o carpeta en concreto
            // si ponemos delante de un comando de sh # no correra ese comando
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }

        stage('Test') {
                    agent {
                        docker {
                            image 'mcr.microsoft.com/playwright:v1.58.0-noble'
                            reuseNode true
                        }
                    }

                    steps {
                    //test - f es para buscar un archivo o carpeta en concreto
                    // si ponemos delante de un comando de sh # no correra ese comando
                        sh '''
                            npm install -g serve
                            serve -s build
                            npx playwright test
                        '''
                    }
                }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}