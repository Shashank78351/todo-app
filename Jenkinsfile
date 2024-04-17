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
        stage('sonarqube'){
            steps{
                withSonarQubeEnv('SonarQube'){
                    mvn clean verify sonar:sonar \
                    -Dsonar.projectKey=todo-sample \
                    -Dsonar.projectName='todo-sample' \
                    -Dsonar.host.url=http://linuxappvm.eastus.cloudapp.azure.com:9000 
                }
            }
        }
    }
}
