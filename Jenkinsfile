pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        git(url: 'https://github.com/avoinea/jenkins-pipeline-test.git', branch: 'master')
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