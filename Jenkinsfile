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
            steps {
                sh '''
                    echo "Test stage"
                    test -f build/index.html && echo "index.html exists" || echo "index.html does not exist"
                    # Se o junit.xml já existe, mova-o para o diretório test-results
                    mkdir -p test-results
                    if [ -f junit.xml ]; then
                        mv junit.xml test-results/junit.xml
                    else
                        echo "Warning: junit.xml not found in the workspace"
                    fi
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
