
pipeline {
    agent any

    stages {

        stage('Git Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Maven Build') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -f dockerfile -t ecommerce-order-management:v2 .'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl delete job ecommerce-job --ignore-not-found=true'
                bat 'kubectl apply -f k8s/job.yaml'
            }
        }
    }

    post {
        success {
            echo 'Ecommerce application deployed successfully!'
        }

        failure {
            echo 'Ecommerce pipeline failed. Check the console output.'
        }
    }
}
