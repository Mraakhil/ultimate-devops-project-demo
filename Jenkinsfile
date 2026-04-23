pipeline{
    agent any
    parameters{
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Branch to build')
        choice(name: 'SERVICE_NAME', choices: ['accounting', 'ad', 'cart', 'checkout', 'currency','email', 'payment', 'product-catalog', 'recommendation', 'shipping'], description: 'Choose the service to build')
    }
    stages{
        stage('pull code'){
            steps{
                git url: 'https://github.com/Mraakhil/ultimate-devops-project-demo.git', branch: 'main'
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
                    if (params.SERVICE_NAME == 'accounting') {
                        dir('ultimate-devops-project-demo/src/accounting') {
                            sh 'docker build -t accounting:latest .' 
                            sh 'docker tag accounting:latest 240828341590.dkr.ecr.ap-south-1.amazonaws.com/accounting:latest'
                            sh 'docker push 240828341590.dkr.ecr.ap-south-1.amazonaws.com/accounting:latest'
                        }
                    }
                    else if (params.SERVICE_NAME == 'ad') {
                        dir('ultimate-devops-project-demo/src/ad') {
                            sh 'docker build -t ad:latest .' 
                            sh 'docker tag ad:latest 240828341590.dkr.ecr.ap-south-1.amazonaws.com/ad:latest'
                            sh 'docker push 240828341590.dkr.ecr.ap-south-1.amazonaws.com/ad:latest'
                        }
                    }
                    else if (params.SERVICE_NAME == 'cart') {
                        dir('ultimate-devops-project-demo/src/cart') {
                            sh 'docker build -t cart:latest .' 
                            sh 'docker tag cart:latest 240828341590.dkr.ecr.ap-south-1.amazonaws.com/cart:latest'
                            sh 'docker push 240828341590.dkr.ecr.ap-south-1.amazonaws.com/cart:latest'
                        }
                    }
                    else if (params.SERVICE_NAME == 'checkout') {
                        dir('ultimate-devops-project-demo/src/checkout') {
                            sh 'docker build -t checkout:latest .' 
                            sh 'docker tag checkout:latest 240828341590.dkr.ecr.ap-south-1.amazonaws.com/checkout:latest'
                            sh 'docker push 240828341590.dkr.ecr.ap-south-1.amazonaws.com/checkout:latest'
                        }
                    }
                    else if (params.SERVICE_NAME == 'currency') {
                        dir('ultimate-devops-project-demo/src/currency') {
                            sh 'docker build -t currency:latest .' 
                            sh 'docker tag currency:latest 240828341590.dkr.ecr.ap-south-1.amazonaws.com/currency:latest'
                            sh 'docker push 240828341590.dkr.ecr.ap-south-1.amazonaws.com/currency:latest'
                        }
                    }
                    else if (params.SERVICE_NAME == 'email') {
                        dir('ultimate-devops-project-demo/src/email') {
                            sh 'docker build -t email:latest .' 
                            sh 'docker tag email:latest 240828341590.dkr.ecr.ap-south-1.amazonaws.com/email:latest'
                            sh 'docker push 240828341590.dkr.ecr.ap-south-1.amazonaws.com/email:latest'
                        }
                    }
                    else if (params.SERVICE_NAME == 'payment') {
                        dir('ultimate-devops-project-demo/src/payment') {
                            sh 'docker build -t payment:latest .' 
                            sh 'docker tag payment:latest 240828341590.dkr.ecr.ap-south-1.amazonaws.com/payment:latest'
                            sh 'docker push 240828341590.dkr.ecr.ap-south-1.amazonaws.com/payment:latest'
                        }
                    }
                    else if (params.SERVICE_NAME == 'product-catalog') {
                        dir('ultimate-devops-project-demo/src/product-catalog') {
                            sh 'docker build -t product-catalog:latest .' 
                            sh 'docker tag product-catalog:latest 240828341590.dkr.ecr.ap-south-1.amazonaws.com/product-catalog:latest'
                            sh 'docker push 240828341590.dkr.ecr.ap-south-1.amazonaws.com/product-catalog:latest'
                        }
                    }
                    else if (params.SERVICE_NAME == 'recommendation') {
                        dir('ultimate-devops-project-demo/src/recommendation') {
                            sh 'docker build -t recommendation:latest .' 
                            sh 'docker tag recommendation:latest 240828341590.dkr.ecr.ap-south-1.amazonaws.com/recommendation:latest'
                            sh 'docker push 240828341590.dkr.ecr.ap-south-1.amazonaws.com/recommendation:latest'
                        }
                    }
                    else if (params.SERVICE_NAME == 'shipping') {
                        dir('ultimate-devops-project-demo/src/shipping') {
                            sh 'docker build -t shipping:latest .' 
                            sh 'docker tag shipping:latest 240828341590.dkr.ecr.ap-south-1.amazonaws.com/shipping:latest'
                            sh 'docker push 240828341590.dkr.ecr.ap-south-1.amazonaws.com/shipping:latest'
                        }
                    }
                }
            }
        }
    }
}
