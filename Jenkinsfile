pipeline {
    agent any

    tools {
        nodejs 'Node 22'   // must match the name in Jenkins Tools
    }

    environment {
        CI = 'true'
    }

    options {
        timestamps()
        timeout(time: 20, unit: 'MINUTES')
        buildDiscarder(logRotator(numToKeepStr: '15'))
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install') {
            steps {
                sh 'node --version'
                sh 'npm --version'
                sh 'npm ci'
            }
        }

        stage('Lint') {
            steps {
                sh 'npm run lint'
            }
        }

        stage('Build') {
            steps {
                // Runs: tsc -b && vite build  →  creates dist/
                sh 'npm run build'
            }
        }

        stage('Archive') {
            steps {
                // Vite output folder is dist/ (not build/)
                archiveArtifacts artifacts: 'dist/**', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Vite React app linted and built successfully.'
        }
        failure {
            echo 'Pipeline failed — open Console Output for the first red stage.'
        }
        always {
            cleanWs()   // needs Workspace Cleanup plugin
        }
    }
}