pipeline {
  agent {
    node {
      label 'maven'
    }
  } 
   environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "https"
        NEXUS_URL = "lic-nexus.apps.cluster-aea6.aea6.example.opentlc.com"
        NEXUS_REPOSITORY = "rhpam"
        NEXUS_CREDENTIAL_ID = "nexus-credentials"
        GIT_URL ="https://github.com/poonam2309"
        GIT_PROJECT ="helloTask"
    }
  stages {
    stage('Containerfilecopied') {
        steps {
            script {
                      echo "Tesing Pipeline"
                      sh 'mvn --version'
                      sh 'java -version'
                      sh 'pwd'
                      sh 'ls -lart'
                      echo "inside git clone"
                      sh 'git clone ${GIT_URL}/${GIT_PROJECT}'
                      sh 'ls -la ${GIT_PROJECT}'
                      echo "new env ====== ${NEXUS_URL}"
                     // sh 'mvn versions:set -DremoveSnapshot'
              //sh 'mvn --settings settings.xml clean package -Dorg.slf4j.simpleLogger.defaultLogLevel=error ' 
                      sh 'cd ${GIT_PROJECT}' 
                      sh 'mvn --settings settings.xml clean package --file ${GIT_PROJECT}/pom.xml -Dorg.slf4j.simpleLogger.defaultLogLevel=warn' 
                   }
                }
             }
    stage('Publish to Nexus Repository Manager') {
            steps {
                script {
                    echo "inside nexus push"
                    sh 'ls -la' 
                    pom = readMavenPom file: "${GIT_PROJECT}/pom.xml";
                    filesByGlob = findFiles(glob: "${GIT_PROJECT}/target/*.jar");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    echo '$artifactPath'
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                      echo "*** File: ${artifactPath}, group: ${pom.groupId}, artifact: ${pom.artifactId} packaging: ${pom.packaging}, version: ${pom.version}"
                         nexusArtifactUploader(
                             nexusVersion: 'nexus3',
                             protocol: 'https',
                              nexusUrl: NEXUS_URL,
                               groupId: pom.groupId,
                               version: pom.version,
                               repository: NEXUS_REPOSITORY,
                               credentialsId: NEXUS_CREDENTIAL_ID,
                     artifacts: [
                       [artifactId: pom.artifactId,
                        classifier: 'metadata',
                        file: filesByGlob[0].path,
                        type: 'jar']
                            ]
                         );
                      sh 'file uploaded into nexus successfully'
                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
                }
            }
        }
    stage('kieServerDeploy'){
      steps {
        script {
          echo "Deploying projects in Kie-server"
          sh 'curl -v -k -u "adminUser:5G3XHvX4" -X PUT "https://lic-kieserver.apps.cluster-aea6.aea6.example.opentlc.com/services/rest/server/containers/test" --data-binary "@kie-container.xml" -H "Content-Type: application/xml"'
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
