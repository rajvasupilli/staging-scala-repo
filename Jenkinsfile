pipeline {
    agent any
    
    parameters {
          text(name: 'DEV_ACCOUNT_ID', defaultValue: '406370147020', description: 'Enter the AWS Account ID of Dev Environment')
          text(name: 'STAGING_ACCOUNT_ID', defaultValue: '406370147020', description: 'Enter the AWS Account ID of Staging Environment')
        string(name: 'REGION', defaultValue: 'us-east-1', description: 'Enter the Region')
        string(name: 'IMAGE_TAG', defaultValue: 'latest', description: 'Enter the tag pertaining to the ECR Image')
        string(name: 'DEV_REPO_NAME', defaultValue: 'dev-scala-image-repo', description: 'Enter the AWS ECR Repo name pertaining to Dev Environment')
        string(name: 'STAGING_REPO_NAME', defaultValue: 'staging-scala-image-repo', description: 'Enter the AWS ECR Repo name pertaining to Staging Environment')        
    }
    
    stages {
        stage('Install Pre-requisites') {
            steps {
                echo 'Installing the prerequisites!!!'
                sh '''
                      aws cloudformation create-stack --stack-name ecs-stack --template-body file://create-staging-ecr.yml --capabilities CAPABILITY_NAMED_IAM
                   '''
            }
        }

       stage('Push image from Dev to Staging ECR') {
            steps {
                echo 'Build,Tag and Push the Docker Image into the ECR'
                sh """ aws ecr get-login-password --region ${params.REGION} | sudo docker login --username AWS --password-stdin 8${params.DEV_ACCOUNT_ID}.dkr.ecr.${params.REGION}.amazonaws.com
                       sudo docker pull ${params.DEV_ACCOUNT_ID}.dkr.ecr.${params.REGION}.amazonaws.com/${params.DEV_REPO_NAME}:${params.IMAGE_TAG} 
                       sudo docker tag ${params.DEV_REPO_NAME}:${params.IMAGE_TAG}  ${params.STAGING_ACCOUNT_ID}.dkr.ecr.${params.REGION}.amazonaws.com/${params.STAGING_REPO_NAME}:${params.IMAGE_TAG}
                       sudo docker push ${params.STAGING_ACCOUNT_ID}.dkr.ecr.${params.REGION}.amazonaws.com/${params.STAGING_REPO_NAME}:${params.IMAGE_TAG}     
                   """
            }
        }
    }
}
