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
            script{
               def userHost = "ec2-user@172.31.2.185"
               def tomcatBin = "ec2-user@172.31.2.185 /opt/tomcat8/bin"
	   		   sshagent(['tomcat-dev']) {
                  // copy war file to tomcat webapps
                  sh "scp -o StrictHostKeyChecking=no target/*.war ${userHost}:/opt/tomcat8/webapps/my-app.war"
                  // stop and start tomcat
                  sh "ssh ${tomcatBin}/shutdown.sh"
                  sh "ssh ${tomcatBin}/startup.sh"
                     		           }

	              }
	        
                  }
	       }         
	   }	   
	
	post {
                success {
    // jenkins job succcessfullu build
		
		mail bcc: '', body: '''Hi 
               Welcome to jenkins job email alerts

              Thanks
               Teja''', cc: '', from: '', replyTo: '', subject: 'Jenkins Pipeline Job', to: 'teja.thotatt@gmail.com'
	
	
	
                        }
              }
		
		
}



























