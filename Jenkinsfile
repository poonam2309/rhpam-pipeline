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
    stage('kieServerDeploy'){
      steps {
        script {
          echo "Deploying projects in Kie-server"
          sh 'curl -v -u "admin:admin" -X PUT "http://localhost:8080/kie-server/services/rest/server/containers/test_1.0.0-SNAPSHOT" --data-binary "@kie-container.xml" -H "Content-Type: application/xml"'
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
