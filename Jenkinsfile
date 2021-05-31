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

stage ('Publish_Artifacts') {
//This will upload generated zip file to artifactory repo-spring-petclinic
     rtUpload (
             buildName: JOB_NAME,
                buildNumber: BUILD_NUMBER,
          serverId: 'JFrog',
           spec: '''{
              "files": [
                 {
                    "pattern": "zip/*.zip",
                    "target": "SDP-maven-Local",
                    "recursive": "false"
                 }
                        ]
                     }''')
    }
	stage ('Publish build info') {
//This will publish the build info in json format to artifactory
            //steps {
                rtPublishBuildInfo (
                    buildName: JOB_NAME,
                    buildNumber: BUILD_NUMBER,
                    serverId: 'JFrog'
                                   )
                  //}
                             }
	
	stage('clean workspace') {
       sh 'mvn clean'
            sh 'rm -rf ${WORKSPACE}/zip/*'
        }
	
	//stage('CD') {
	//sshPublisher(publishers: [sshPublisherDesc(configName: 'ACS', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook -i playbooks/hosts playbooks/Example.yml --user ansible', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//home/ansible', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
	//sshPublisher(publishers: [sshPublisherDesc(configName: 'Ansible control server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook -i playbooks/hosts playbooks/Example.yml --user ansible', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
	//sshPublisher(publishers: [sshPublisherDesc(configName: 'Ansible control server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook -i /home/ansible/playbooks/hosts /home/ansible/playbooks/Example.yml --user ansible', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '**')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
	
	
	//sshPublisher(publishers: [sshPublisherDesc(configName: 'Ansible control server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook -i /home/ansible/playbooks/hosts /home/ansible/playbooks/Example.yml ', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//home/ansible', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'Jenkinsfile')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
	//sshPublisher(publishers: [sshPublisherDesc(configName: 'Ansible control server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//home/ansible', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'Jenkinsfile'), sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook -i /home/ansible/playbooks/hosts /home/ansible/playbooks/Example.yml ', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
	
	
	//}
	
	stage('cd'){
		sh 'ssh ansible@3.142.205.70'
		sh 'cd ~/playbooks/'
		//echo "changed to playbooks path"
		sh 'ansible-playbook -i hosts Example.yml'
		//echo "executed yml file"
	}
	
	
}
