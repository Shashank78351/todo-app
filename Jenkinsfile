pipeline {
    agent any
    tools {
        maven "maven"
    }
    environment{
        SCANNER_HOME= tool 'sonar-scanner'
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
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName='todo-sample' -Dsonar.projectKey=todo-sample \
                           -Dsonar.java.binaries=. '''
                //  sh "mvn clean verify sonar:sonar \
                //         -Dsonar.projectKey=todo-sample \
                //         -Dsonar.projectName='todo-sample' \
                //         -Dsonar.host.url=http://linuxappvm.eastus.cloudapp.azure.com:9000 \
                //         -Dsonar.token=sqp_a1830fc740c06afc59952d2977170121489ef1b3"
                }
            }
        }
        stage('Docker Build') {
            steps {
                withDockerRegistry(credentialsId: '9da214ad-553c-443b-a1c4-169a8a78cfe7', url: 'registry.gitlab.com') {
                  sh 'docker build -t registry.gitlab.com/smr1234/sample-web . '
                }
                
            }
        }
    }
}
