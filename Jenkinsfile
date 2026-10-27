
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/ayeshanadeem1328/TaskFlow-DevOps.git'
            }
        }

        stage('Check Laravel Project') {
            steps {
                sh '''
                    echo "Checking Laravel project..."
                    test -f taskflow/artisan
                    test -f taskflow/composer.json
                    php --version
                    composer --version
                    echo "Laravel files found successfully."
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                dir('taskflow') {
                    sh 'composer install --no-interaction --prefer-dist'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t taskflow:1.0 ./taskflow'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    kubectl apply -f kubernetes/taskflow-deployment.yaml
                    kubectl apply -f kubernetes/taskflow-service.yaml
                    kubectl rollout status deployment/taskflow --timeout=120s
                '''
            }
        }
    }

    post {
        success {
            echo 'TaskFlow pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the stage logs.'
        }
    }
}
