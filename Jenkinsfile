String credentialsId = 'jonyjalfon94-dockerhub'

try {
  stage('checkout') {
    node {
      cleanWs()
      checkout scm
    }
  }
  // Run Docker build init
  stage('Build Docker Images') {
    node {
        docker.withRegistry('', 'credentialsId') {
        docker.build("eshop-web", "./src/Web/Dockerfile") 
        docker.build("eshop-api", "./src/Web/Dockerfile") 
      }
    }
  }

  if (env.BRANCH_NAME == 'master') {
    stage('Publish Images') {
      node {
       docker.withRegistry('', 'credentialsId') {
           
        }
      }
    }
  }
  currentBuild.result = 'SUCCESS'
}
catch (org.jenkinsci.plugins.workflow.steps.FlowInterruptedException flowError) {
  currentBuild.result = 'ABORTED'
}
catch (err) {
  currentBuild.result = 'FAILURE'
  throw err
}
finally {
  if (currentBuild.result == 'SUCCESS') {
    currentBuild.result = 'SUCCESS'
  }
}












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
