node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def mvn = tool 'MAVEN_HOME';
    withSonarQubeEnv() {
      bat "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=testproject"
    }
  }
  
stage("UPLOAD TO JFROG")
      {
    git url: 'https://github.com/AlphaRomeoX1/demo-project.git'

    // Get Artifactory server instance, defined in the Artifactory Plugin administration page.
    def server = Artifactory.server "jfrog"

    // Read the upload spec and upload files to Artifactory.
    def downloadSpec =
            '''{
            "files": [
                {
                    "pattern": "libs-snapshot-local/*.zip",
                    "target": "dependencies/",
                    "props": "p1=v1;p2=v2"
                }
            ]
        }'''

    def buildInfo1 = server.download spec: downloadSpec

    // Read the upload spec which was downloaded from github.
    def uploadSpec =
            '''{
            "files": [
                {
                    "pattern": "resources/Kermit.*",
                    "target": "libs-snapshot-local",
                    "props": "p1=v1;p2=v2"
                },
                {
                    "pattern": "resources/Frogger.*",
                    "target": "libs-snapshot-local"
                }
            ]
        }'''

    // Upload to Artifactory.
    def buildInfo2 = server.upload spec: uploadSpec

    // Merge the upload and download build-info objects.
    buildInfo1.append buildInfo2

    // Publish the build to Artifactory
    server.publishBuildInfo buildInfo1
}
      }
  
}
