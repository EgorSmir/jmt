node {
   def mvnHome
   stage('Preparation') {
      git 'https://github.com/EgorSmir/jmt.git'
      mvnHome = tool 'Maven3.5'
   }
   stage('Build') {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
   }
   stage('Results') {
      archive 'target/*.jar'
   }
   stage('Publish') {
        nexusArtifactUploader artifacts: [[artifactId: 'nexus-artifact-uploader', classifier: '', file: 'testpipeline/target/my-app-1.0-SNAPSHOT.jar', type: 'jar']], credentialsId: '7fa0c97c-1838-431a-a9c8-73c59bc47ac2', groupId: 'group', nexusUrl: '192.168.56.12', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-releases', version: '1'
   }
}
