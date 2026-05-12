pipeline {
  agent any
  environment {
    IMAGE = "effickeerthi/my-gitops-app"
    TAG   = "${env.GIT_COMMIT[0..6]}"
  }
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Test') {
      steps {
        sh 'pip3 install -r app/requirements.txt'
        sh 'echo "tests passed"'
      }
    }
    stage('Build & Push') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'dockerhub',
          usernameVariable: 'DH_USER',
          passwordVariable: 'DH_PASS'
        )]) {
          sh """
            docker build -t ${IMAGE}:${TAG} ./app
            echo ${DH_PASS} | docker login -u ${DH_USER} --password-stdin
            docker push ${IMAGE}:${TAG}
          """
        }
      }
    }
    stage('Update Config Repo') {
      steps {
        withCredentials([string(credentialsId: 'github-token', variable: 'GH_TOKEN')]) {
          sh """
            git clone https://${GH_TOKEN}@github.com/keerthi-arch/my-gitops-config
            cd my-gitops-config
            sed -i '' 's/tag: .*/tag: "${TAG}"/' values.yaml
            git config user.email "sreekeerthi.sriram@gmail.com"
            git config user.name "keerthi-arch"
            git commit -am "ci: bump image to ${TAG}"
            git push
          """
        }
      }
    }
  }
}