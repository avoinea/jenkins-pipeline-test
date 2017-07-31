pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/avoinea/jenkins-pipeline-test.git'
      }
    }
    stage('Tests') {
      steps {
        sh '''echo "$(ls -l)"
echo "=========="
echo $WORKSPACE
echo "=========="'''
      }
    }
  }
}