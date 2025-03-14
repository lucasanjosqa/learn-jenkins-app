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
            steps {
                sh '''
                    echo "Test stage"
                    if test -f build/index.html; then
                        echo "index.html exists"
                    else
                        echo "index.html does not exist"
                        exit 1
                    fi
                    npm test || exit 1
                    mkdir -p test-results
                    if [ -f junit.xml ]; then
                        mv junit.xml test-results/junit.xml
                    elif [ ! -f test-results/junit.xml ]; then
                        echo "Error: test-results/junit.xml not found"
                        exit 1
                    fi
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