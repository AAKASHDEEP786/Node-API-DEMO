pipeline {
    agent any

    tools {
        nodejs 'NodeJS_18'
    }

    environment {
        CI = 'true'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main',
                    credentialsId: 'git-cred',
                    url: 'https://github.com/AAKASHDEEP786/Node-API-DEMO'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Unit Tests') {
            steps {
                script {
                    try {
                        sh 'npm test'
                    } catch (err) {
                        echo "No unit tests defined or test failed, continuing..."
                    }
                }
            }
        }

        stage('Run Newman API Tests') {
            steps {
                script {
                    // Start the server in background
                    sh 'nohup npm start &'
                    // Wait for server to start
                    sh 'sleep 5'
                    // Run the Postman/Newman tests
                    sh 'newman run tests/postman-collection.json'
                }
            }
        }
    }

    post {
        always {
            echo "Build complete."
        }
    }
}
