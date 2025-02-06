pipeline {
    agent any
    environment {
        AWS_REGION = 'us-east-1' 
    }
    stages {
        stage('Set AWS Credentials') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'Jenkins' 
                ]]) {
                    sh '''
                    echo "AKIAZZA6M2ZULUVR45O4: $AWS_ACCESS_KEY_ID"
                    aws sts get-caller-identity
                    '''
                }
            }
        }
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/BigN2233/jenkins/' 
            }
        }
        stage('Initialize Terraform') {
            steps {
                sh '''
                terraform init
                '''
            }
        }
        stage('Plan Terraform') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'Jenkins'
                ]]) {
                    sh '''
                    export AKIAZZA6M2ZULUVR45O4=$AWS_ACCESS_KEY_ID
                    export quZQoH9FjyS6wrsz/dVW4LLFcxCFhWkd0xDZQzs+=$AWS_SECRET_ACCESS_KEY
                    terraform plan -out=tfplan
                    '''
                }
            }
        }
        stage('Apply Terraform') {
            steps {
                input message: "Approve Terraform Apply?", ok: "Deploy"
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'Jenkins'
                ]]) {
                    sh '''
                    export AKIAZZA6M2ZULUVR45O4=$AWS_ACCESS_KEY_ID
                    export quZQoH9FjyS6wrsz/dVW4LLFcxCFhWkd0xDZQzs+=$AWS_SECRET_ACCESS_KEY
                    terraform apply -auto-approve tfplan
                    '''
                }
            }
        }
    }
    post {
        success {
            echo 'Terraform deployment completed successfully!'
        }
        failure {
            echo 'Terraform deployment failed!'
        }
    }
}
