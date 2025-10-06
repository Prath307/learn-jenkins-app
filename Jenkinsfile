pipeline {
    agent any
/* 
    stages {
        stage('Hello') {
            steps {
                echo 'Hello world'
            }
        }
    }
*/

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
                    echo "Hello World from Jenkinsfile"
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
                   sh '''
                        test -f build/index.html
                        # checks the index file exists or
                        npm test
                    '''
            }
        }
    }

/*
        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.55.0-noble'
                    reuseNode true
                }
            }
            steps {
                   sh '''
                        npm install serve
                        node_modules/.bin/serve -s build &
                        sleep 10
                        npx playwright test
                    '''
            }
        }
    }
    */

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }

}