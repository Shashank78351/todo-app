pipeline {
    agent any
    tools {
        maven "maven"
        dockerTool 'docker'
    }
    environment {
        DOCKER_REGISTRY = 'https://registry.gitlab.com'
        // Define Docker image tag
        // DOCKER_TAG = 'latest'
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
                script {
                    // Build Docker image
                    docker.build("smr1234/sample-web:latest")
                }
            }
        }
         stage('Docker push') {
            steps {
                script {
                    // Push Docker image to registry
                    docker.withRegistry("${DOCKER_REGISTRY}", '9da214ad-553c-443b-a1c4-169a8a78cfe7') {
                        def dockerImage = docker.image("smr1234/sample-web:latest")
                        dockerImage.push("${env.BUILD_NUMBER}")
                        dockerImage.push("latest")
                    }
                }
            }
        }
    }
}
