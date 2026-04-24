pipeline {
    agent any
    
    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Branch to build')
        choice(name: 'SERVICE_NAME', 
               choices: ['accounting', 'ad', 'cart', 'checkout', 'currency', 'email', 'payment', 'product-catalog', 'recommendation', 'shipping'], 
               description: 'Choose the service to build')
    }
    
    environment {
        // Variables keep your pipeline clean and easy to update later
        AWS_REGION   = "ap-south-1"
        ECR_REGISTRY = "240828341590.dkr.ecr.ap-south-1.amazonaws.com"
        REPO_NAME    = "vprofileappimg"
    }

    stages {
        stage('Pull Code') {
            steps {
                git url: 'https://github.com/Mraakhil/ultimate-devops-project-demo.git', 
                    branch: "${params.BRANCH_NAME}"
            }
        }

        stage('Docker Login') {
            steps {
                sh "aws ecr get-login-password --region ${env.AWS_REGION} | docker login --username AWS --password-stdin ${env.ECR_REGISTRY}"
            }
        }

        stage('Build and Push Service') {
            steps {
                script {
                    def service = params.SERVICE_NAME
                    def imageTag = "${env.ECR_REGISTRY}/${env.REPO_NAME}:${service}-latest"
                    
                    // Run from root context (.) so Docker can see /src and /pb
                    sh "docker build -t ${service}:latest -f src/${service}/Dockerfile ." 
                    
                    sh "docker tag ${service}:latest ${imageTag}"
                    sh "docker push ${imageTag}"
                }
            }
        }
    
    post {
        always {
            // 5. Clean up the local image to free up Jenkins disk space
            script {
                echo "Cleaning up local workspace images..."
                sh "docker rmi -f \$(docker images -q ${params.SERVICE_NAME}:latest) || true"
            }
        }
    }
}
