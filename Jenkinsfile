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
                    docker.build("335871625378.dkr.ecr.us-east-1.amazonaws.com/hamids-netflix:latest")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 335871625378.dkr.ecr.us-east-1.amazonaws.com/hamids-netflix:latest'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/hamid-netflix', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://335871625378.dkr.ecr.us-east-1.amazonaws.com/hamids-netflix', 'ecr:us-east-1:hamid-ecr') {
                    // build image
                    def myImage = docker.build("335871625378.dkr.ecr.us-east-1.amazonaws.com/hamids-netflix:latest")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}
