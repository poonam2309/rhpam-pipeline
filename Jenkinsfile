pipeline {
  agent any
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
          sh 'sleep 2m'
          //sh 'docker build -t helloworld:latest /tmp/workspace/dockerbuild/'
          
        }
      }
    }
  }
}
