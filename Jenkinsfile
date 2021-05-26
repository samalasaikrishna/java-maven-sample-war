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
	
	stage ('sonar') {
    // performing sonarqube analysis with "withSonarQubeENV(<Name of Server configured in Jenkins>)"
    withSonarQubeEnv('SonarQube_scan') {
      // requires SonarQube Scanner for Maven 3.2+
      sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar'
    }
  }
	stage ('creating_zip') {
//this will create a zip file out of currently builded jar/ear/war file and stores zip in workspace
fileOperations([fileZipOperation(folderPath: 'target/Example-0.0.1-SNAPSHOT.war', outputFolderPath: 'zip')])
	}
	
	stage ('renaming_zip') {
//this will rename the zip file appending the build number
               // sh 'cd ${WORKSPACE}/zip_test'
                sh 'mv ${WORKSPACE}/zip/*.zip ${WORKSPACE}/zip/${BUILD_NUMBER}-Example.zip'
}
	
	
	
}
