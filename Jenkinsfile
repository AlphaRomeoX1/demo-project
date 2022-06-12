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
  
  rtUpload (
    serverId: 'Artifactory-1',
    specPath: 'path/to/spec/relative/to/workspace/spec.json',
    failNoOp: true,
 
    // Optional - Associate the uploaded files with the following custom build name and build number.
    // If not set, the files will be associated with the default build name and build number (i.e the
    // the Jenkins job name and number).
    buildName: 'BUILD-1',
    buildNumber: '1.0',
    // Optional - Only if this build is associated with a project in Artifactory, set the project key as follows.
    project: 'my-project-key'
)
  
}
