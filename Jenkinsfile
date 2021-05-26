node {
   stage('SCM') {
	  git branch: 'master', url: 'https://github.com/samalasaikrishna/java-maven-sample-war.git'
   }
  stage ('build the packages') {
	  sh 'mvn install'
          }
  stage ('archiving') {
     //archiving the artifacts
     archiveArtifacts 'target/*.war'
      // archiveArtifacts 'target/*.war'
     }
}
