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
                    docker.build("565027876289.dkr.ecr.us-east-1.amazonaws.com/hamid-netflix:v1")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 565027876289.dkr.ecr.us-east-1.amazonaws.com/hamid-netflix:v1'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/hamid-netflix:v1', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://565027876289.dkr.ecr.us-east-1.amazonaws.com/hamid-netflix:v1', 'ecr:us-east-1:hamid-ecr') {
                    // build image
                    def myImage = docker.build("565027876289.dkr.ecr.us-east-1.amazonaws.com/hamid-netflix:v1")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}
