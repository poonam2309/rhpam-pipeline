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
                      sh 'pwd'
                      sh 'ls -lrt'
                      sh 'mvn clean package' 
                      sh 'cat /etc/redhat-release'
                      sh 'sudo bash'
                    }
                }
             }
    stage('ProjectBuild') {
      steps {
        script { 
          echo "Hello Poonam"
          //sh 'docker build -t helloworld:latest /tmp/workspace/dockerbuild/'
          
        }
      }
    }
  }
}
