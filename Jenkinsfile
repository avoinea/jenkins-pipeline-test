pipeline {
  agent any
  stages {
    stage('Tests') {
      steps {
        parallel(
          "WWW": {
            node(label: 'docker') {
              sh '''docker run --name="$BUILD_TAG" -i --net=host eeacms/www:devel bash -c 'bin/develop up && bin/test -v -vv -s eea.alchemy'
docker rm -v "$BUILD_TAG"'''
            }
            
            
          },
          "Plone4": {
            node(label: 'docker') {
              sh '''docker run -i --net=host --name="$BUILD_TAG" -e BUILDOUT_DEVELOP=src/eea.alchemy -e BUILDOUT_EGGS=eea.alchemy eeacms/plone-test bin/test -s eea.alchemy
docker rm -v $BUILD_TAG'''
            }
            
            
          },
          "Coverage": {
            node(label: 'docker') {
              sh '''mkdir -p xmltestreport
docker run -i --net=host --name=$BUILD_TAG eeacms/www:devel bash -c "bin/coverage run bin/xmltestreport -s eea.alchemy && bin/report xml --include=*eea/alchemy*"
docker cp $BUILD_TAG:/plone/instance/parts/xmltestreport/testreports xmltestreport/
docker rm -v $BUILD_TAG
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