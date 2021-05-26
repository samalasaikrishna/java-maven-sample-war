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
	
	
	
	
}
