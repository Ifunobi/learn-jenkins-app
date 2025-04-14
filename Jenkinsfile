pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:23-alpine'
                    reuseNode true
                }
            }
            environment {
                NODE_OPTIONS = '--openssl-legacy-provider'
            }
            steps {
                sh '''
                    ls -la
                    # Print the current working directory
                    pwd
                    # Print the Node.js version
                    node -v
                    # Print the npm version
                    npm -v
                    #!/usr/bin/env bash
                    set -euo pipefail

                    # Install dependencies
                    npm install

                    # Build the project
                    npm run build
                '''
            }
        } // Close Build stage

        stage('Test') {
            echo 'Running unit tests...'
            agent {
                docker {
                    image 'node:23-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    #!/usr/bin/env bash
                    set -euo pipefail

                    ls -la
                    # Print the current working directory
                    pwd
                    # Print the Node.js version
                    node -v
                    # Print the npm version
                    npm -v

                    test -f build/index.html

                    # Run tests
                    npm test
                '''
            }
        } // Close Test stage

        stage('E2E') {
            echo 'Running end-to-end tests...'
            agent {
                docker {
                    image 'node:23-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    #!/usr/bin/env bash
                    set -euo pipefail
                    npm install -g serve
                    npm -s build
                    npx playwright test

                '''
            }
        } // Close E2E stage
    } // Close stages
    post {
        always {
            archiveArtifacts artifacts: 'build/**/*', fingerprint: true
            junit 'test-results/junit.xml'
        }
        success {
            echo 'Build and tests completed successfully!'
        }
        failure {
            echo 'Build or tests failed.'
        }
    }
} // Close pipeline