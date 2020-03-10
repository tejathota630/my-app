pipeline {
   agent any
       tools {
               maven 'maven3'
             }
	   stages{
	 
	       stage('Gitcheckout') {
		   
		     steps {  
			          git credentialsId: 'teja', url: 'https://github.com/tejathota630/my-app'
					  
				    }
	                            }
	   stage('Maven Build/Package') {
		   
		     steps {  
			          sh 'mvn clean package'
					  
				    }
	                            }
								
	         }
	
        }
