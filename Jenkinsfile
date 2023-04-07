pipeline {
  environment {
    imagename = "brindilsz/demo:web-vote-app"
    registryCredential = 'registry'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/jenarutamara/swarm-microservice-demo-v1.git', branch: 'redis', credentialsId: 'github'])
 
      }
    }
    stage('Building image') {
   sh """
        docker build -t redis01 .
      """
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $imagename:$BUILD_NUMBER"
         sh "docker rmi $imagename:latest"
 
      }
    }
  }
}
