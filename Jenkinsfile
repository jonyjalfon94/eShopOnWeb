pipeline {
  environment {
    web_imagename = "jonyjalfon94/ehop-web"
    api_imagename = "jonyjalfon94/ehop-api"
    registryCredential = 'jonyjalfon94-dockerhub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/jonyjalfon94/eShopOnWeb.git', branch: 'master', credentialsId: 'GithubCredentials'])

      }
    }
    stage('Building images') {
      steps{
        script {
          webImage = docker.build web_imagename, "./src/Web/Dockerfile"
          apiImage = docker.build api_imagename, "./src/PublicApi/Dockerfile"
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            webImage.push("$BUILD_NUMBER")
             webImage.push('latest')
            apiImage.push("$BUILD_NUMBER")
             apiImage.push('latest')
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $web_imagename:$BUILD_NUMBER"
         sh "docker rmi $web_imagename:latest"
          sh "docker rmi $api_imagename:$BUILD_NUMBER"
         sh "docker rmi $api_imagename:latest"

      }
    }
  }
}
