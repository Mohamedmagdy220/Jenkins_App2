pipeline {
  agent any

  environment {
    IMAGE_NAME = 'mohamed2200/jenkins-app:latest'
  }

  stages {
    stage('Clone Repo') {
      steps {
        echo 'Repo already cloned by Jenkins – skipping manual git clone'
      }
    }

    stage('Run Unit Tests') {
      steps {
        sh 'mvn test'
      }
    }

    stage('Build App') {
      steps {
        sh 'mvn package -DskipTests'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh "docker build -t $IMAGE_NAME ."
      }
    }

    stage('Push Docker Image') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
          sh "docker push $IMAGE_NAME"
        }
      }
    }

    stage('Delete Local Docker Image') {
      steps {
        sh "docker rmi $IMAGE_NAME"
      }
    }

    stage('Update deployment.yaml') {
      steps {
        sh "sed -i 's|image: .*|image: $IMAGE_NAME|' k8s/deployment.yaml"
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f k8s/deployment.yaml'
      }
    }
  }

  post {
    always {
      echo 'Pipeline finished!'
    }
    success {
      echo 'Deployment successful!'
    }
    failure {
      echo 'Deployment failed!'
    }
  }
}
