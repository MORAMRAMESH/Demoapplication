pipeline{
   agent any
   tools {
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
	stage('Maven Build'){
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
}
