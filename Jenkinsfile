pipeline {
    agent any
    tools {
        maven "maven"
    }

    stages{
    //     stage('Build Artifact'){
    //         steps{
    //             sh "mvn clean package -DskipTests=true"
    //             archive 'target/*.jar'
    //         }

    //     }
    //     stage('test'){
    //         steps{
    //             sh "mvn test"
    //             junit 'target/surefire-reports/*.xml'
    //         }

    //     }
    //     stage('sonarqube'){
    //         steps{
    //             withSonarQubeEnv('SonarQube'){
    //               sh "mvn clean verify sonar:sonar \
    //                     -Dsonar.projectKey=todo-sample \
    //                     -Dsonar.projectName='todo-sample' \
    //                     -Dsonar.host.url=http://linuxappvm.eastus.cloudapp.azure.com:9000 \
    //                     -Dsonar.token=sqp_a1830fc740c06afc59952d2977170121489ef1b3"
    //             }
    //         }
    //     }
        stage('Docker Build') {
             steps {
                script{
                       app= docker.build("sample-web")
                    }
                    
                }
            }
        stage('Docker push') {
            steps {
                 script{
                    withDockerRegistry(credentialsId: '9da214ad-553c-443b-a1c4-169a8a78cfe7', toolName: 'docker', url: 'registry.gitlab.com') {
                      app.push(latest)     
                    }
                 }
                
            }
        }
    }
}
