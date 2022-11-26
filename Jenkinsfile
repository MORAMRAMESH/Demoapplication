pipeline{
   agent any
   stages{
	stage('git checkout'){
	   steps{
             git branch: 'main', url: 'https://github.com/MORAMRAMESH/Demoapplication.git'
	   }

	}
        stage('Unit testing'){
           steps{
	      withMaven(maven: 'Maven3') {
              sh 'mvn test'
	      }
           }

        }

   }
}
