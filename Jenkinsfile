pipeline {
    agent any
    
    environment {
        AWS_ACCESS_KEY_ID       = credentials('jenkins-aws-access-key-id')
        AWS_SECRET_ACCESS_KEY   = credentials('jenkins-aws-access-key')
        AWS_S3_BUCKET           = 'demo-ebs-1'
        ARTIFACT_NAME           = 'demo-ebs.jar'
        AWS_EB_APP_NAME         = 'demo-ebs'
        AWS_EB_ENVIRONMENT      = 'demo-ebs-env-1'
        AWS_EB_APP_VERSION      = "${BUILD_ID}"
    }

    stages {
         stage('GitHub') {
             steps {
                 echo '- - - - - - - Connecting to GitHub - - - - - - -'
                 git(
                     url:"https://github.com/Lulilu61/demo-ebs.git",
                     branch: "main"
                     )
             }
             }
            
            stage('Build') {
            steps {
                echo '- - - - - - - Building App - - - - - - -'
                sh 'mvn clean package -DskipTests=true'
                archive 'target/*.jar'
            }


            }
          stage('Test') {
            steps {
                echo '- - - - - - - Testing App - - - - - - -'
                sh 'mvn test'
                }
                post {
                    success {
                        sh 'aws configure set region eu-west-3'
                        echo '==== Uploading App ===='
                        sh 'aws s3 cp ./target/*.jar s3://$AWS_S3_BUCKET/$ARTIFACT_NAME'
                    }
                }
            }
            
            stage('Deploy') {
            steps {
                echo '- - - - - - - Deploying App - - - - - - - '
                sh 'aws elasticbeanstalk create-application-version --application-name $AWS_EB_APP_NAME --version-label $AWS_EB_APP_VERSION --source-bundle S3Bucket=$AWS_S3_BUCKET,S3Key=$ARTIFACT_NAME'
                sh 'aws elasticbeanstalk update-environment --application-name $AWS_EB_APP_NAME --environment-name $AWS_EB_ENVIRONMENT --version-label $AWS_EB_APP_VERSION'
                }
            }
        }
    }