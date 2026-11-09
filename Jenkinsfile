pipeline{
    agent any
    Environment{
        DOCKER_USER = "ranguharini"
        DOCKER_PASSWORD = "harini@2005"
        IMAGE_NAME = "ranguharini/exam"
        IMAGE_TAG = "latest"
    }
    stages{
        stage('checkout'){
            steps{
                bat'checkout scm'
            }
        }
        stage('Build Docker Image'){
            steps{
                bat"docker build -t %IMAGE_NAME%:%IMAGE_TAG% ."
            }
        }
        stage('Docker Login'){
            steps{
                bat"docker login -u %DOCKER_USER% -p %DOCKER_PASSWORD%"
            }
        }
        stage('Push Docker Image'){
            steps{
                bat'docker tag %IMAGE_NAME%:%IMAGE_TAG% %DOCKER_USER%/%IMAGE_NAME%:%IMAGE_TAG%'
                bat"docker push %IMAGE_NAME%:%IMAGE_TAG%"
            }
        }
        stage('Deploy to Kubernetes'){
            steps{
                bat"kubectl apply -f deployment.yaml --validate=false"
                bat"kubectl apply -f service.yaml"
            }
        }
        stage('Verify Deployment'){
            steps{
                bat'kubectl get deployments'
                bat"kubectl get pods"
                bat"kubectl get services"
            }
        }
    }
    post{
        success{
            echo 'Pipeline completed successfully.'
        }
        failure{
            echo 'Pipeline failed.'
        }
    }
}