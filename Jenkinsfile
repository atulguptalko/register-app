pipeline {
    agent { label 'Jenkin-Agent' }
    tools {
        jdk 'java17'
        maven 'Maven3'
    }
    envoironment {
	    APP_NAME = "register-app-pipeline"
	    RELEASE= "1.0.0"
	    DOCKER_USER= "atulguptalko"
	    DOCKER_PASSWORD= 'dockerhub'
	    IMAGE_NAME= "${DOCKER_USER}" + "/" + "${APP_NAME}"
	    IMAGE_TAG= "${RELEASE}-${BUILD_NUMBER}"
	    JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")
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
}
       stage("Quality Gate"){
           steps {
               script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
                }	
            }

        }
	   stage("Build & Push Docker Image") {
            steps {
                script {
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }

       }
     }
}
