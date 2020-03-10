pipeline {
   agent any
       tools {
               maven 'maven3'
             }
	   stages{
		   
	   stage('Maven Build/Package') {
		   
		     steps {  
			          sh 'mvn clean package'
					  
		           }
	                                }

	   stage('Nexus Deploy') {
		   
		     steps { 
		script{
               def pomFile = readMavenPom file: 'pom.xml'
               def version = pomFile.version
               def nexusRepo = version.endsWith("SNAPSHOT") ? "my-app-snapshot" : "my-app-release"
			           nexusArtifactUploader artifacts: [[artifactId: 'my-app', classifier: '', file: 'target/my-app.war', type: 'war']], 
					   credentialsId: 'Nexus3', 
					   groupId: 'in.javahome', 
					   nexusUrl: '172.31.47.0:8081', 
					   nexusVersion: 'nexus3', 
					   protocol: 'http', 
					   repository: nexusRepo, 
					   version: version
					   }
					  
		           }
	                                }								
				 
	   stage('Deploy-tomcat') {
		   
		     steps {  
			        sshagent(['tomcat-dev']) {
						//copy war file to tomcat webapps
                       sh "scp -o  StrictHostKeyChecking =no target/*.war ec2-user@172.31.2.185:/opt/tomcat8/webapp/my-app.war"
					   //stop and start tomcat
					   sh"ssh ec2-user@172.31.2.185 /opt/tomcat8/bin/shudown.sh"
					   sh"ssh ec2-user@172.31.2.185 /opt/tomcat8/bin/startup.sh"
                                             }
					  
		           }

	        }
	
           }
