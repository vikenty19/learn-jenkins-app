pipeline {
    agent any

    stages {
        stage('Build') {
            agent{
                docker{
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
                '''}
        }
        stage('E2E'){
            agent{
                docker{
                    image 'docker pull mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }
            steps{
                sh '''
                   npm install -g serve
                   serve -s build
                   npx playwright test
                '''
            }
        
    }

        stage('Test'){
                agent{
                    docker{
                        image 'node:18-alpine'
                        reuseNode true
                    }
                }
                steps{
                    sh '''
                        test -f build/index.html
                        npm test
                    '''
                }
            
        }
    }
    post{
        always{
            junit 'test-results/junit.xml'
        }
    }
    
}
