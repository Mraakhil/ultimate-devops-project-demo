pipeline{
    agent any
    parameters{
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Branch to build')
        choice(name: 'SERVICE_NAME', choices: ['accounting', 'ad', 'cart', 'checkout', 'currency','email', 'payment', 'product-catalog', 'recommendation', 'shipping'], description: 'Choose the service')
    }
    stages{
        stage('pull code'){
            steps{
                git url: 'https://github.com/Mraakhil/ultimate-devops-project-demo.git', branch: "${params.BRANCH_NAME}"
            }
        }

        stage('docker login') {
            steps {
                sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 240828341590.dkr.ecr.ap-south-1.amazonaws.com'
            }
        }

        stage('Build Service') {
            steps {
                script {
                    // This moves you into the specific folder
                    dir("src/${params.SERVICE_NAME}") {
                        
                        // Now that you are inside, use "." for the Dockerfile path
                        sh "docker build -t ${params.SERVICE_NAME}:latest -f Dockerfile ." 
                        
                        sh "docker tag ${params.SERVICE_NAME}:latest 240828341590.dkr.ecr.ap-south-1.amazonaws.com/vprofileappimg:${params.SERVICE_NAME}-latest"
                        sh "docker push 240828341590.dkr.ecr.ap-south-1.amazonaws.com/vprofileappimg:${params.SERVICE_NAME}-latest"
                    }
                }
            }
        }
    }
}
