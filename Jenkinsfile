pipeline {
    agent any
    tools {
        maven "maven"
        dockerTool 'docker'
    }
    environment {
        DOCKER_REGISTRY = 'https://index.docker.io/v1/'
        // YQ_VERSION = "4.12.0"
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
                    docker.build("smr123/sample-web:latest")
                }
            }
        }
         stage('Docker push') {
            steps {
                script {
                    // Push Docker image to registry
                    docker.withRegistry("${DOCKER_REGISTRY}", 'db28354f-484c-4bda-9aac-975c35bf0c2c') {
                        def dockerImage = docker.image("smr123/sample-web:latest")
                        dockerImage.push("${env.BUILD_NUMBER}")
                        dockerImage.push("latest")
                    }
                }
            }
        }
        stage('Deploying') {
            steps {
                script {
                    sh "curl -LO https://github.com/mikefarah/yq/releases/download/4.12.0/yq_linux_amd64"
                    sh "sudo mv yq_linux_amd64 /usr/local/bin/yq"
                    sh "chmod +x /usr/local/bin/yq"
                    sh "yq e -i '.spec.template.spec.containers[0].image=\"sample-web:${env.BUILD_NUMBER}\"' kube/deployment.yaml"
                }
            }
        }
    }
}
