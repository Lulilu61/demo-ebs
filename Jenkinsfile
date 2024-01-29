pipeline {
    agent any

    stages {
        // stage('GitHub') {
        //     steps {
        //         echo '- - - - - - - Connecting to GitHub - - - - - - -'
        //         git(
        //             url:"https://github.com/Lulilu61/demo-ebs.git",
        //             branch: "main"
        //             )
               
        //     }
        //     }
            
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
                always {
                junit 'target/surefire-reports/*.xml'
                jacoco execPattern: 'target/jacoco.exec'
                }
            }
            }
            stage('Depliy') {
            steps {
                echo '- - - - - - - Deploying App - - - - - - - '
               
            }
            }
        }
    }
