pipeline {
    agent any 
    stages {
        stage('Install Pre-requisites') {
            steps {
                echo 'Installling the prerequisites!!!'
                sh '''
                      #bash prereq.sh
                   '''
            }
        }

       stage('Push image from Dev to Staging ECR') {
            steps {
                echo 'Build,Tag and Push the Docker Image into the ECR'
                sh ''' aws ecr get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin 206406752358.dkr.ecr.us-east-1.amazonaws.com
                       sudo docker pull 206406752358.dkr.ecr.us-east-1.amazonaws.com/dev-scala-image-repo:latest
                       #docker build -t staging-scala-image-repo .
                       sudo docker tag dev-scala-image-repo:latest 206406752358.dkr.ecr.us-east-1.amazonaws.com/staging-scala-image-repo:latest
                       sudo docker push 206406752358.dkr.ecr.us-east-1.amazonaws.com/staging-scala-image-repo:latest    
                   '''
            }
        }
    }
}
