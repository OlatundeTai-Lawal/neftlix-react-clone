pipeline {
    agent any
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages{
        // NPM dependencies
        stage('pull npm dependencies') {
            steps {
                sh 'npm install'
            }
        }
       stage('build Docker Image') {
            steps {
                script {
                    // build image
                    docker.build("544795736365.dkr.ecr.us-east-1.amazonaws.com/netflix-clone-app")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 544795736365.dkr.ecr.us-east-1.amazonaws.com/netflix-clone-app:latest'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://544795736365.dkr.ecr.us-east-1.amazonaws.com/netflix-clone-app', 'ecr:us-east-1:Ola-AWS') {
                    // build image
                    def myImage = docker.build("544795736365.dkr.ecr.us-east-1.amazonaws.com/netflix-clone-app:latest")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}
