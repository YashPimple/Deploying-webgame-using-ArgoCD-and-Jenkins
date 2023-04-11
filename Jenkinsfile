pipeline {
  agent{
      docker {
      image 'docker:latest'
      registryUrl 'https://index.docker.io/v1/'
      args '-v /var/run/docker.sock:/var/run/docker.sock'
    }
  }
  stages {
     stage('Checkout') {
      steps {
        sh 'echo passed'
        //git branch: 'main', url: 'https://github.com/iam-veeramalla/Jenkins-Zero-To-Hero.git'
      }
     }
     stage('Build and Push Docker Image') {
      environment {
        DOCKER_IMAGE = "yashpimple22/web-game:${BUILD_NUMBER}"
        // DOCKERFILE_LOCATION = "java-maven-sonar-argocd-helm-k8s/spring-boot-app/Dockerfile"
        REGISTRY_CREDENTIALS = credentials('docker-cred')
      }
      steps {
        script {
            sh 'docker build -t ${DOCKER_IMAGE} .'
            def dockerImage = docker.image("${DOCKER_IMAGE}")
            docker.withRegistry('https://index.docker.io/v1/', "docker-cred") {
                dockerImage.push()
            }
        }
      }
    }
     stage('Updating deployment.yaml'){
       environment {
            GIT_REPO_NAME = "Project-4"
            GIT_USER_NAME = "YashPimple"
       }
      steps {
        withCredentials([string(credentialsId: 'github', variable: 'GITHUB_TOKEN')]) {
          sh '''
            git config user.email "yashpimple22@gmail.com"
            git config user.name "YashPimple"
            BUILD_NUMBER=${BUILD_NUMBER}
            sed -i "s/latest/${BUILD_NUMBER}/g" Manifests/deployment.yaml
            git add Manifests/deployment.yaml
            git commit -m "Update deployment image to version ${BUILD_NUMBER}"
            git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} HEAD:main
          '''
        }
      }
  }
  
  }
} 
