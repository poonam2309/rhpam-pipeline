pipeline {
  agent {
    node {
      label 'maven'
    }
  } 
  stages {
    stage('Containerfilecopied') {
        steps {
            script {
                      echo "Tesing Pipeline"
                      sh 'mvn --version'
                      sh 'java -version'
                      sh 'pwd'
                      sh 'ls -lrt'
                      sh 'mvn clean package --settings settings.xml' 
                      sh 'cat /etc/redhat-release'
                    }
                }
             }
    stage('ProjectBuild') {
      steps {
        script { 
          echo "Hello Poonam"
          sh 'sleep 2m'
          //sh 'docker build -t helloworld:latest /tmp/workspace/dockerbuild/'
          
        }
      }
    }
  }
}
