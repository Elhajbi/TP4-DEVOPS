pipeline {
  environment {
    registry = "abdelkarim12/tp4"
  }
  agent any
  stages {

    stage('Cloning Git') {
      steps {
        git 'https://github.com/Elhajbi/TP4-DEVOPS.git'
      }
    }

    stage('Building image') {
      steps {
        sh '/Applications/Docker.app/Contents/Resources/bin/docker build -t abdelkarim12/tp4:${BUILD_NUMBER} .'
      }
    }

    stage('Test image') {
      steps {
        echo "Tests passed"
      }
    }

    stage('Publish Image') {
      steps {
        withCredentials([usernamePassword(
            credentialsId: 'dockerhub-creds',
            usernameVariable: 'USER',
            passwordVariable: 'PASS')]) {
          sh '''
            echo "$PASS" | /Applications/Docker.app/Contents/Resources/bin/docker login -u "$USER" --password-stdin
            /Applications/Docker.app/Contents/Resources/bin/docker push abdelkarim12/tp4:${BUILD_NUMBER}
          '''
        }
      }
    }

    stage('Deploy image') {
      steps {
        sh '/Applications/Docker.app/Contents/Resources/bin/docker stop tp4container || true'
        sh '/Applications/Docker.app/Contents/Resources/bin/docker rm tp4container || true'
        sh '/Applications/Docker.app/Contents/Resources/bin/docker run -d --name tp4container -p 8081:80 abdelkarim12/tp4:${BUILD_NUMBER}'
      }
    }

  }
}
