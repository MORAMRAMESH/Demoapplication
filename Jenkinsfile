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
						def nexusRepo = readPomVersion.version.endsWith("SNAPSHOT")? "demoapp-snapshot" : "demoapp-release"
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
		stage('Docker Image Build'){
			steps{
				script{
					sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
					sh 'docker image tag $JOB_NAME:v1.$BUILD_ID moramramesh/$JOB_NAME:v1.$BUILD_ID'
					sh 'docker image tag $JOB_NAME:v1.$BUILD_ID moramramesh/$JOB_NAME:latest'
				}
			}
		}
		stage('Docker image push to DockerHub'){
			steps{
				script{
					withCredentials([string(credentialsId: 'Docker_Hub_Cred', variable: 'Hub-cred')]) {
						sh 'docker login -u moramramesh -p $(Docker_Hub_Cred)'
						sh 'docker image push moramramesh/$JOB_NAME:v1.$BUILD_ID'
						sh 'docker image push moramramesh/$JOB_NAME:v1.latest'
					}
				}
			}
		}
   	}
}
