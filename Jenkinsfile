pipeline {
  agent {
    node {
      label 'maven'
    }
  } 
   environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "https"
        NEXUS_URL = "https://lic-nexus.apps.cluster-aea6.aea6.example.opentlc.com"
        NEXUS_REPOSITORY = "rhpam"
        NEXUS_CREDENTIAL_ID = "nexus-credentials"
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
    }  //Project build state completed
    
    stage("Publish to Nexus Repository Manager") {
            steps {
                script {
                    pom = readMavenPom file: "pom.xml";
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );
                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
                }
            }
        }
  }
}
