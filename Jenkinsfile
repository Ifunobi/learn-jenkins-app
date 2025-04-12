pipeline {
    agent any

    stages {
        stage('Without Docker') {
            steps {
                cleanWs()
                echo '"Without Docker"'
            }
        }
        
        stage('With Docker') {
            agent {
                docker {
                    image 'node:23-alpine'
                    reuseNode true
                }
            }
            steps {
                echo '"With Docker"'
                '''
                npm --version
                npm start
                '''
            }
        }
    }
}
