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
        stage('updating manifest') {
            environment {
                GIT_REPO = "sample-web"
                GIT_USER = "smr1234"
            }
            steps {
                withCredentials([gitUsernamePassword(credentialsId: '9da214ad-553c-443b-a1c4-169a8a78cfe7', gitToolName: 'Default')]) {
                    sh """
                        git config user.email "sm00776153@techmahindra.com"
                        git config user.name "smr1234"
                        git checkout main
                        BUILD_NUMBER=${env.BUILD_NUMBER}
                        sed -i "s/latest/${BUILD_NUMBER}/g" kube/deployment.yml > kube/deployment_tmp.yml
                        cp kube/deployment_tmp.yml kube/deployment.yml
                        rm kube/deployment_tmp.yml
                        cat kube/deployment.yml
                        git add kube/deployment.yml 
                        git status
                        git commit -m "update deployment image to version ${BUILD_NUMBER}"
                        git push https://glpat-2_AbyCe2Bz_fwRFsiyZi@gitlab.com/${GIT_USER}/${GIT_REPO} HEAD:main

                    """
                }
            }
        }
    }
}
