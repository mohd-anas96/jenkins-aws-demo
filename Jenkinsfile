pipeline {
    agent any

    environment {
        S3_BUCKET = "jenkins-demo-artifacts-12345"
        LAMBDA_NAME = "jenkins-demo-lambda"
        ARTIFACT = "lambda.zip"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Package') {
            steps {
                sh '''
                zip -r $ARTIFACT app.py
                '''
            }
        }

        stage('Upload to S3') {
            steps {
                sh '''
                aws s3 cp $ARTIFACT s3://$S3_BUCKET/$ARTIFACT
                '''
            }
        }

        stage('Deploy to Lambda') {
            steps {
                sh '''
                aws lambda update-function-code \
                --region ap-south-1 \
                --function-name $LAMBDA_NAME \
                --s3-bucket $S3_BUCKET \
                --s3-key $ARTIFACT
                '''
            }
        }
    }
}
