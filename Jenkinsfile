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
        }
    }
}
