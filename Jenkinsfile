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
    
     stage('Build') {
      steps {
        sh 'docker build -f "src/Web/Dockerfile" -t "${web_imagename}:latest" .'
        sh 'docker build -f "src/PublicApi/Dockerfile" -t "${api_imagename}:latest" .'
      }
    }
    
    stage('Publish') {
      when {
        branch 'master'
      }
      steps {
        withDockerRegistry([ credentialsId: 'registryCredential', url: "" ]) {
          sh 'docker push ${web_imagename}:latest'
          sh 'docker push ${api_imagename}:latest'
       }
    }
  }
}
