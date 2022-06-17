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
          
       def downloadSpec = """{
 "files": [
  {
      "pattern": "jfrogrepo/*.zip",
      "target": "jfrogrepo/"
    }
 ]
}"""
        
server.download spec: downloadSpec
        def uploadSpec = """{
  "files": [
    {
      ""pattern": "jfrogrepo/*.zip",
      "target": "jfrogrepo/"
    }
 ]
}"""
server.upload spec: uploadSpec
        
        def buildInfo1 = server.download downloadSpec
def buildInfo2 = server.upload uploadSpec
buildInfo1.append buildInfo2
server.publishBuildInfo buildInfo1
def buildInfo = Artifactory.newBuildInfo()
server.download spec: downloadSpec, buildInfo: buildInfo
server.upload spec: uploadSpec, buildInfo: buildInfo
server.publishBuildInfo buildInfo
        if (buildInfo.getDependencies().size() > 0) {
    def localPath = buildInfo.getDependencies()[0].getLocalPath()
    def remotePath = buildInfo.getDependencies()[0].getRemotePath()
    def md5 = buildInfo.getDependencies()[0].getMd5()
    def sha1 = buildInfo.getDependencies()[0].getSha1()
}
 
if (buildInfo.getArtifacts().size() > 0) {
    def localPath = buildInfo.getArtifacts()[0].getLocalPath()
    def remotePath = buildInfo.getArtifacts()[0].getRemotePath()
    def md5 = buildInfo.getArtifacts()[0].getMd5()
    def sha1 = buildInfo.getArtifacts()[0].getSha1()
}
        def buildInfo = Artifactory.newBuildInfo()
buildInfo.project = 'my-jfrog-project-key'
server.publishBuildInfo buildInfo
        def buildInfo = Artifactory.newBuildInfo()
buildInfo.env.capture = true
        def buildInfo1 = Artifactory.newBuildInfo()
buildInfo1.name = 'my-app-linux'
buildInfo1.number = '1'
server.publishBuildInfo buildInfo1
 
def buildInfo2 = Artifactory.newBuildInfo()
buildInfo2.name = 'my-app-windows'
buildInfo2.number = '1'
server.publishBuildInfo buildInfo2
        
        def finalBuildInfo = Artifactory.newBuildInfo()
server.buildAppend(finalBuildInfo, 'my-app-linux', '1')
server.buildAppend(finalBuildInfo, 'my-app-windows', "1")
server.publishBuildInfo finalBuildInfo
        def promotionConfig = [
    // Mandatory parameters
    'targetRepo'         : 'libs-prod-ready-local',
 
    // Optional parameters
 
    // The build name and build number to promote. If not specified, the Jenkins job's build name and build number are used
    'buildName'          : buildInfo.name,
    'buildNumber'        : buildInfo.number,
    // Only if this build is associated with a project in Artifactory, set the project key as follows.
    'project': 'my-project-key',
    // Comment and Status to be displayed in the Build History tab in Artifactory
    'comment'            : 'this is the promotion comment',
    'status'             : 'Released',
    // Specifies the source repository for build artifacts.
    'sourceRepo'         : 'libs-staging-local',
    // Indicates whether to promote the build dependencies, in addition to the artifacts. False by default
    'includeDependencies': true,
    // Indicates whether to copy the files. Move is the default
    'copy'               : true,
    // Indicates whether to fail the promotion process in case of failing to move or copy one of the files. False by default.
    'failFast'           : true
]
 
// Promote build
server.promote promotionConfig
  
}
