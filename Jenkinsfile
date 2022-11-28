pipeline{
   	agent any
   	tools{
        maven 'Maven3'
   	}
	stages{
		stage('git checkout'){
	   		steps{
             		git branch: 'main', url: 'https://github.com/MORAMRAMESH/Demoapplication.git'
			}
		}
        stage('Unit testing'){
        		steps{
	      			sh 'mvn test'
	   		}   
        	}
		stage('Integration testing'){
           		steps{
              			sh 'mvn verify -DskipUnitTests'
           		}
       		}
		stage('Maven Building'){
	   		steps{
	      			sh 'mvn clean install'
	   		}
		}
		stage('Static code analysis'){
	   		steps{
	    			script{
	     				withSonarQubeEnv(credentialsId: 'sonar-api'){
	     					sh 'mvn clean package sonar:sonar'
	     				}
	    			} 
	   		}
        	}
		stage('Quality gate status'){
	   		steps{
	      			script{
                			waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api' 
	      			}
	  		}
		}
		stage('upload artifact to nexus'){
	   		steps{
	     			script{
						def readPomVersion = readMavenPom file: 'pom.xml'
						def nexusRepo = readPomVersion.version.endsWith("SNAPSHOT")? "dempapp-snapshot" : "demoapp-release"
	       				nexusArtifactUploader artifacts: 
	       				[
	        				[
		   					artifactId: 'springboot', 
		   					classifier: '', 
		   					file: 'target/Uber.jar', 
		   					type: 'jar'
						]
	       				], 
	           			credentialsId: 'nexus-auth', 
		   			groupId: 'com.example', 
		   			nexusUrl: '44.201.13.127:8081/', 
		   			nexusVersion: 'nexus3', 
		   			protocol: 'http', 
		   			repository: nexusRepo, 
		   			version: "${readPomVersion.version}"
	     		}
	  		}
       	}
   	}
}
