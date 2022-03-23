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
                      sh 'curl -s https://lic-nexus.apps.cluster-aea6.aea6.example.opentlc.com/repository/rhpam/com/epgs/test/maven-metadata.xml'
                      //sh 'curl -s https://lic-nexus.apps.cluster-aea6.aea6.example.opentlc.com/repository/rhpam/com/epgs/test/maven-metadata.xml |grep "<release>.*</release>" | sed -e "s#\(.*\)\(<release>\)\(.*\)\(</release>\)\(.*\)#\3#g"'
                      echo "new env ====== ${NEXUS_URL}"
                     // sh 'mvn versions:set -DremoveSnapshot'
              //sh 'mvn --settings settings.xml clean package -Dorg.slf4j.simpleLogger.defaultLogLevel=error ' 
                      sh 'mvn --settings settings.xml clean package -Dorg.slf4j.simpleLogger.log.org.apache.maven.cli.transfer.Slf4jMavenTransferListener=warn' 
                   }
                }
             }
    stage('Publish to Nexus Repository Manager') {
            steps {
                script {
                    echo "inside nexus push"
                    sh 'ls -la' 
                    pom = readMavenPom file: "pom.xml";
                    filesByGlob = findFiles(glob: "target/*.jar");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    echo '$artifactPath'
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader{
                          nexusVersion(${NEXUS_VERSION})
                          protocol(${NEXUS_PROTOCOL})
                          nexusUrl(${NEXUS_URL})
                          groupId(${pom.groupId})
                          version(${pom.version})
                          repository(${NEXUS_REPOSITORY})
                          credentialsId(${NEXUS_CREDENTIAL_ID})
                              artifact {
                                artifactId(pom.artifactId)
                                classifier('debug')
                                type(pom.packaging)
                                file(${filesByGlob[0].name})
                              }
                        }
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
          sh 'curl -v -k -u "adminUser:5G3XHvX4" -X PUT "https://lic-kieserver.apps.cluster-aea6.aea6.example.opentlc.com/services/rest/server/containers/test_1.0.0" --data-binary "@kie-container.xml" -H "Content-Type: application/xml"'
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
