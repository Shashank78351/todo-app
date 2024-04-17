pipeline {
    agent any
    tools {
        maven "maven 3.8.6"
    }

    stages{
        stage('Build Artifact'){
            steps{
                sh "mvn clean package -DskipTests=true"
                archive 'target/*.jar'
            }

        }
        stage('test'){
            steps{
                sh "mvn test"
            }
            post{
                always{
                    junit 'target/surfire-reports/*.xml'
                }
            }
        }
    }
}
