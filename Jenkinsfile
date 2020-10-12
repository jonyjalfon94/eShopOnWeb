String credentialsId = 'jonyjalfon94-dockerhub'

try {
  stage('checkout') {
    node {
      cleanWs()
      checkout scm
    }
  }
  // Run Docker build
  stage('Build Docker Images') {
    node {
        docker.withRegistry('', credentialsId) {
        //sh "docker build --pull -t jonyjalfon94/eshop-web:${BUILD_NUMBER} -f src/Web/Dockerfile ."
        //sh "docker build --pull -t jonyjalfon94/eshop-api:${BUILD_NUMBER} -f src/Web/Dockerfile ."
          sh "docker-compose build"
          sh "docker-compose up -d"
      }
    }
  }
    if (env.BRANCH_NAME == 'master') {
      stage('Publish Images') {
        node {
          docker.withRegistry('', credentialsId) {
            //sh "docker push jonyjalfon94/eshop-web:${BUILD_NUMBER}"
            //sh "docker push jonyjalfon94/eshop-web:latest"
            //sh "docker push jonyjalfon94/eshop-api:${BUILD_NUMBER}"
            //sh "docker push jonyjalfon94/eshop-api:latest"
            sh "docker-compose push"
          }
        }
      }
    }
 post {
        success {
            echo 'Run eshop-infrastructure pipeline!'
            build job: 'eshop-infra'
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
