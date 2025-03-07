pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
    }

    stages {
        stage('Install Dependencies') {
            steps {
                dir('cdk') {
                    sh 'npm install'
                }
            }
        }

        stage('CDK Bootstrap') {
            steps {
                withAWS(credentials: 'aws-credentials', region: "${AWS_REGION}") {
                    dir('cdk') {
                        sh 'cdk bootstrap'
                    }
                }
            }
        }

        stage('CDK Synth') {
            steps {
                withAWS(credentials: 'aws-credentials', region: "${AWS_REGION}") {
                    dir('cdk') {
                        sh 'cdk synth'
                    }
                }
            }
        }

        stage('CDK Deploy') {
            steps {
                withAWS(credentials: 'aws-credentials', region: "${AWS_REGION}") {
                    dir('cdk') {
                        sh 'cdk deploy --require-approval never'
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
