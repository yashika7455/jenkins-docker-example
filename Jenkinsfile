pipeline {
  agent { label 'jenkins-agent' }
  tools {
    jdk 'Java17'
    maven 'Maven'
  }
  environment {
    DOCKER_IMAGE = 'yashika7455/jenkins-docker-example:latest'
    DOCKER_REGISTRY_CREDENTIALS = 'dockerhub'
  }
  stages {
    stage("Cleanup Workspace") {
      steps {
        cleanWs()
      }
    }
    stage("Checkout from SCM") {
      steps {
        git branch: 'main', credentialsId: 'github', url: 'https://github.com/yashika7455/jenkins-docker-example.git'
      }
    }
    stage("Build Application") {
      steps {
        sh "mvn clean package"
      }
    }
    stage("Test Application") {
      steps {
        sh "mvn test"
      }
    }
    stage("Build and Push Docker Image") {
      steps {
        script {
          withDockerRegistry([credentialsId: "${DOCKER_REGISTRY_CREDENTIALS}", url: 'https://index.docker.io/v1/']) {
            sh "docker login -u ${env.DOCKERHUB_USER} -p ${env.DOCKERHUB_PASS}"
            sh "docker build -t ${DOCKER_IMAGE} ."
            sh "docker push ${DOCKER_IMAGE}"
          }
        }
      }
    }
  }
  post {
    success {
      echo "Pipeline executed successfully!"
    }
    failure {
      echo "Pipeline failed!"
    }
  }
}
