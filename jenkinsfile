pipeline {
    agent any

    stages {

        stage('Checkout Source Code') {
            steps {
                echo '========== CHECKOUT =========='
                checkout scm
            }
        }

        stage('Build Podman Image') {
            steps {
                echo '========== BUILD =========='
                sh 'podman build -t my-nginx-app:latest .'
            }
        }

        stage('Stop Old Container') {
            steps {
                echo '========== CLEANUP =========='
                sh 'podman rm -f my-nginx-container || true'
            }
        }

        stage('Run New Container') {
            steps {
                echo '========== DEPLOY =========='
                sh 'podman run -d --name my-nginx-container -p 8080:80 my-nginx-app:latest'
            }
        }

        stage('Verify Deployment') {
            steps {
                echo '========== VERIFY =========='
                sh 'podman ps'
            }
        }
    }

    post {

        success {
            echo '========================================='
            echo 'Pipeline completed successfully!'
            echo 'Application deployed on http://localhost:8080'
            echo '========================================='
        }

        failure {
            echo '========================================='
            echo 'Pipeline failed!'
            echo 'Check the Console Output for errors.'
            echo '========================================='
        }

        always {
            echo 'Cleaning up workspace...'
            cleanWs()
        }
    }
}
