pipeline {
    agent any

    stages {
        stage('Jenkinsfile') {
            steps {
                sh ''' 
                    'Hello World from Jenkinsfile'
                    jenkins --version
                '''
            }
        }
    }
}