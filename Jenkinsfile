pipeline {
  agent any
  stages {
    stage('Tests') {
      steps {
        parallel(
          "WWW": {
            node(label: 'docker') {
              sh '''CONTAINER_NAME="$BUILD_TAG-www"
docker run -i --net=host --name=$CONTAINER_NAME eeacms/www:devel bash -c 'bin/develop up && bin/test -v -vv -s eea.alchemy'
docker rm -v $CONTAINER_NAME'''
            }
            
            
          },
          "Plone4": {
            node(label: 'docker') {
              sh '''CONTAINER_NAME="$BUILD_TAG-plone4"
docker run -i --name=$CONTAINER_NAME --net=host -e BUILDOUT_DEVELOP=src/eea.alchemy -e BUILDOUT_EGGS=eea.alchemy eeacms/plone-test bin/test -s eea.alchemy
docker rm -v $CONTAINER_NAME'''
            }
            
            
          },
          "Coverage": {
            node(label: 'docker') {
              sh '''mkdir -p xmltestreport
CONTAINER_NAME="$BUILD_TAG-coverage"
docker run -i --net=host --name=$CONTAINER_NAME eeacms/www:devel bash -c "bin/coverage run bin/xmltestreport -s eea.alchemy && bin/report xml --include=*eea/alchemy*"
docker cp $CONTAINER_NAME:/plone/instance/parts/xmltestreport/testreports/ xmltestreport/
docker rm -v $CONTAINER_NAME
'''
              junit '**/xmltestreport/*.xml'
            }
            
            
          }
        )
      }
    }
    stage('Code analysis') {
      steps {
        node(label: 'docker') {
          git 'https://github.com/eea/eea.alchemy.git'
          sh '''echo "$pwd"
echo "ls -al"
echo "$BUILD_TAG"
'''
        }
        
      }
    }
  }
}