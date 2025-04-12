pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:23-buster' // Use a Debian-based image with OpenSSL 1.1
                    reuseNode true
                }
            }
            steps {
                cleanWs()
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
                    npm ci

                    # Build the project
                    npm run build
                '''
            }
        }
    }
}
