pipeline {
  agent any
  stages {
    stage('Docker Build') {
      steps {
        sh "docker build -t arunmagi/magi2:${env.BUILD_NUMBER} ."
      }
    }
    stage('Docker Push') {
      steps {
          sh "docker login -u arunamgi -p Arun@ak@99"
          sh "docker push arunmagi/magi2:${env.BUILD_NUMBER}"
        }
    }
    stage('Docker Remove Image') {
      steps {
        sh "docker rmi arunmagi/magi2:${env.BUILD_NUMBER}"
      }
    }
    stage('Apply Kubernetes Files') {
      steps {
          withKubeConfig([credentialsId: 'kubeconfig']) {
          sh 'cat deployment.yaml | sed "s/{{BUILD_NUMBER}}/$BUILD_NUMBER/g" | kubectl apply -f -'
          sh 'kubectl apply -f service.yaml'
        }
      }
  }
}
post {
    success {
      slackSend(message: "Pipeline is successfully completed.")
    }
    failure {
      slackSend(message: "Pipeline failed. Please check the logs.")
    }
}
}
