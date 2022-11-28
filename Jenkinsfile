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
		   			nexusUrl: '3.238.0.150:8081', 
		   			nexusVersion: 'nexus3', 
		   			protocol: 'http', 
		   			repository: 'demoapp-release', 
		   			version: '1.0.0'
	     			}
	  		}
       		}
   	}
}
