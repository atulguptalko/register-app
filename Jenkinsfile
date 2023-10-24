pipeline {
    agent { label 'Jenkin-Agent' }
    tools {
        jdk 'java17'
        maven 'Maven3'
    }
     stages{
	     stage('clean workspace'){
		     steps{
		      cleanWs()
		     }
	     }

	     stage('checkout from SCM'){
		     steps{
			git branch: 'main', credentialsId: 'github', url: 'https://github.com/atulguptalko/register-app.git'
		     }
	     }

	     stage("Bulid Application"){
		     steps {
			     sh "mvn clean package"
		     }
	     }

	     stage("Test Application"){
		     steps {
			     sh "mvn test"
		     }
	     }
	     stage("SonarQube Analysis"){
		     steps {
			script {
			 withSonarQubeEnv(credentialsID: 'jenkins-sonarqube-token'){
				 sh "mvn sonar:sonar"
			 }
		}
	}
	     stage("Quality Gate"){
		     steps {
			     script {
				    waitForQuailtyGate abortPipeline: false, credentialsID: 'jenkins-sonarqube-token' 
			     }
		     }
	     }
     }
}
}
