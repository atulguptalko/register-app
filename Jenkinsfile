pipeline {
    agent { label 'Jenkin-Agent' }
    tools {
        jdk 'java17'
        maven 'maven3'
    }
     stages{
	     stage('clean workspace'){
		     steps{
		      cleanWs()
		     }
	     }

	     stage('checkout from SCM'){
		     steps{
			git branch: 'main', credentialsaID: 'github', url: 'https://github.com/atulguptalko/register-app.git'
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
     }
}
