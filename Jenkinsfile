
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

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat 'mvn sonar:sonar -Dsonar.projectKey=ecommerce-order-management'
                }
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
    }
}
