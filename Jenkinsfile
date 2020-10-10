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
        docker.withRegistry('', credentialsId) {
        docker.build("eshop-web", "./src/Web/") 
        docker.build("eshop-api", "./src/Web/") 
      }
    }
  }

  if (env.BRANCH_NAME == 'master') {
    stage('Publish Images') {
      node {
       docker.withRegistry('', credentialsId) {
           
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
