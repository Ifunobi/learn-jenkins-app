pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:23-bullseye'
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


                    # Install dependencies
                    npm install

                    # Build the project
                    npm run build
                '''
            }
        } // Close Build stage
        stage('Test') {
            agent {
                docker {
                    image 'node:23-bullseye'
                    reuseNode true
                }
            }
            steps {
                echo 'Running unit tests...'
                sh '''
                    #!/usr/bin/env bash

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
            agent {
                docker {
                    image 'node:23-alpine'
                    reuseNode true
                }
            }
            steps {
                echo 'Running end-to-end tests...'
                sh '''
                    #!/usr/bin/env bash
                    set -euo pipefail

                    # Install Playwright and its browsers
                    npm install playwright
                    npx playwright install --with-deps

                    # Serve the build directory
                    npm install serve
                    node_modules/.bin/serve -s build &

                    # Wait for the server to start
                    sleep 5

                    # Run Playwright tests
                    npx playwright test --reporter=html
                '''
            }
        } // Close E2E stage
    } // Close stages
    post {
        always {
            archiveArtifacts artifacts: 'build/**/*', fingerprint: true
            junit 'jest-results/junit.xml'
        }
        success {
            echo 'Build and tests completed successfully!'
        }
        failure {
            echo 'Build or tests failed.'
        }
    }
} // Close pipeline