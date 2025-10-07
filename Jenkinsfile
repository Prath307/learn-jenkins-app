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
        /*
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
        */

        stage('Tests') {
            parallel {
                stage('Unit Tests') {
                    agent {
                        docker {
                            image 'node:18-alpine'
                            reuseNode true
                        }
                    }

                    steps {
                        sh '''
                            test -f build/index.html
                            npm test
                        '''
                    }
                }

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
                            npx playwright install
                            node_modules/.bin/serve -s build &
                            # & sign tells to run the site in background and continue execution next
                            sleep 10
                            npx playwright test --reporter=line
                        '''
                    }
                }
            }
        }
    }

  
    post {
        always {
            junit 'jest-results/junit.xml'
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright HTML Report', reportTitles: '', useWrapperFileDirectly: true])
        }
    }

}