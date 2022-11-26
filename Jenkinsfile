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

   }
}
