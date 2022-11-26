pipeline{
   agent any
   tools {
        maven 'Maven 3.8.6'
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
