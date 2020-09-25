pipeline {
    agent any 
    stages {
        stage('Install Pre-requisites') {
            steps {
                echo 'Installling the prerequisites!!!'
                sh '''
                      aws cloudformation update-stack --stack-name ecs-stack --template-body file://create-staging-ecr.yml --capabilities CAPABILITY_NAMED_IAM
                   '''
            }
        }

       stage('Push image from Dev to Staging ECR') {
            steps {
                echo 'Build,Tag and Push the Docker Image into the ECR'
                sh ''' aws ecr get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin 827659600095.dkr.ecr.us-east-1.amazonaws.com
                       sudo docker pull 827659600095.dkr.ecr.us-east-1.amazonaws.com/dev-scala-image-repo:latest
                       sudo docker tag dev-scala-image-repo:latest 827659600095.dkr.ecr.us-east-1.amazonaws.com/staging-scala-image-repo:latest
                       sudo docker push 827659600095.dkr.ecr.us-east-1.amazonaws.com/staging-scala-image-repo:latest    
                   '''
            }
        }
    }
}
