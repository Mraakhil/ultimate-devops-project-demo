pipeline {
    agent any
    
    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'Mraakhil-patch-1', description: 'Branch to build')
        choice(name: 'SERVICE_NAME', 
               choices: ['accounting', 'ad', 'checkout', 'currency', 'email', 'payment', 'product-catalog', 'recommendation', 'shipping', 'frontend-proxy', 'frontend'], 
               description: 'Choose the service to build')
    }
    
    environment {
        AWS_REGION   = "ap-south-1"
        ECR_REGISTRY = "848684726346.dkr.ecr.ap-south-1.amazonaws.com/project/cicdproject"
        REPO_NAME    = "project/cicdproject"
        DOCKER_BUILDKIT = '0'
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
                withCredentials([
                    string(credentialsId: 'AWS_ACCESS_KEY_ID', variable: 'AWS_ACCESS_KEY_ID'),
                    string(credentialsId: 'AWS_SECRET_ACCESS_KEY', variable: 'AWS_SECRET_ACCESS_KEY')
                ]) {
                    sh "aws ecr get-login-password --region ${env.AWS_REGION} | docker login --username AWS --password-stdin ${env.ECR_REGISTRY}"
                }
            }
        }

        stage('Build and Push Service') {
            steps {
                script {
                    def service = params.SERVICE_NAME
                    def imageTag = "${env.ECR_REGISTRY}/${env.REPO_NAME}:${service}-latest"
                    
                    echo "Building ${service} from the repository root context..."
                    sh "docker build -t ${service}:latest -f src/${service}/Dockerfile ." 
                    sh "docker tag ${service}:latest ${imageTag}"
                    sh "docker push ${imageTag}"
                }
            }
        }
    }
    
    post {
        always {
            script {
                echo "Cleaning up local image..."
                sh "docker rmi -f ${params.SERVICE_NAME}:latest || true"
            }
        }
    }
}
