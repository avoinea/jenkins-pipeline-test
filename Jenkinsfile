pipeline {
  agent any
  stages {
    stage('Tests') {
      steps {
        parallel(
          "WWW": {
            node(label: 'docker') {
              sh 'docker run -i --net=host --rm eeacms/www:devel bash -c \'bin/develop up && bin/test -v -vv -s eea.alchemy\''
            }
            
            
          },
          "Plone4": {
            node(label: 'docker') {
              sh 'docker run -i --net=host --rm -e BUILDOUT_DEVELOP=src/eea.alchemy -e BUILDOUT_EGGS=eea.alchemy eeacms/plone-test bin/test -s eea.alchemy'
            }
            
            
          },
          "Coverage": {
            node(label: 'docker') {
              sh '''mkdir -p xmltestreport
setfacl  -R -m u:500:rwX xmltestreport/
setfacl -dR -m u:500:rwX xmltestreport/
docker run -i --net=host --rm -v $(pwd)/xmltestreport:/plone/instance/parts/xmltestreport eeacms/www:devel bash -c "bin/coverage run bin/xmltestreport -s eea.alchemy && bin/report xml --include=*eea/alchemy*"
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
'''
        }
        
      }
    }
  }
}