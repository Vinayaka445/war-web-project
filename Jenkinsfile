pipeline{
    agent any
    
    tools {
        maven 'maven'
    }
    
    environment {
       DOCKER_TAG = getVersion()
   } 
    
    stages{
        stage("SCM Checkout"){
            steps{
               git credentialsId: 'Git_User', url: 'https://github.com/Vinayaka445/war-web-project'
            }
        }
        stage("Maven Build"){
            steps{
                sh "mvn clean package"
            }
        }
        stage("Docker Build"){
            steps{
                sh "docker build . -t vinayaka1995/vinayapp:${DOCKER_TAG}"
            }
        }
        stage("Docker Push"){
            steps{
                withCredentials([string(credentialsId: 'docker-hub', variable: 'DockerHubPWD')]) {
                sh "docker login -u vinayaka1995 -p ${DockerHubPWD}"
                sh "docker push vinayaka1995/vinayapp:${DOCKER_TAG}"
            }
                
            }
        }
    }
}

def getVersion(){
    def commitHash = sh returnStdout: true, script: 'git rev-parse --short HEAD'
    return commitHash
}
