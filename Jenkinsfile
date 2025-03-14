pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            reuseNode true
        }
    }

    stages {
        stage('Build') {
            steps {
                sh '''
                    ls -l
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -l
                '''
            }
        }

        stage('Test') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    #test -f build/index.html
                    npm test
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