pipeline{
    agent{
        label "agent-0"
    }
    tools {
      maven 'mvn-3-5-4'
    }

    environment {
        DOCKER_USER = credentials("docker-username")
        DOCKER_PASS = credentials("docker-password")
    }

    
    stages{
        stage("Build App"){
            steps{
                sh "mvn package install"
            }
        }
        // stage("Test App"){
        //     steps{
        //        sh "mvn test"
        //     }
        // }
        stage("Archive Jar"){
            steps{
                archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
            }
        }
        stage("Build Docker Image"){
            steps{
                sh "docker build -t hassaneid/data-iti:v${IMAGE_NUM} ."
            }
        }
        stage("Docker Login"){
            steps{
                sh "docker login -u ${DOCKER_USER} -p ${DOCKER_PASS}"
            }
        }
        // stage("Docker push"){
        //     steps{
        //         sh "docker push hassaneid/data-iti:v${IMAGE_NUM}"
        //     }
        // }
        stage("Docker deploy"){
            steps{
                sh "docker stop java-app && docker rm java-app"
                sh "docker run -d -p 8090:8090 --name java-app hassaneid/data-iti:v${IMAGE_NUM}"
            }
        }

    }

    post {
        always {
            cleanWs()
        }
        success {
            echo "success"
        }
        failure {
            echo "failure"
        }
    }

}