pipeline {
    agent any
    tools {
        maven "maven"
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
                junit 'target/surefire-reports/*.xml'
            }

        }
    }
}
