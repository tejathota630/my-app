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
			          nexusArtifactUploader artifacts: [[artifactId: 'my-app', classifier: '', file: 'target/my-app.war', type: 'war']], 
					   credentialsId: 'Nexus3', 
					   groupId: 'in.javahome', 
					   nexusUrl: '172.31.47.0:8081', 
					   nexusVersion: 'nexus3', 
					   protocol: 'http', 
					   repository: 'my-app-snapshot', 
					   version: '1.0-SNAPSHOT'
					  
		           }
	                                }								
								
	        }
	
           }
