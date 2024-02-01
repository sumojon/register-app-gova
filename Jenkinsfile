pipeline {
    agent { label 'Jenkins-Agent' }
    tools {
        jdk 'java17'
        maven 'maven3'
    }

 environment {
	    APP_NAME = "register-app-pipeline"
            RELEASE = "1.0.0"
            DOCKER_USER = "dasivardhani"
            DOCKER_PASS = 'dockerhub'
            IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
	        }

	
  stages{
        stage("Cleanup Workspace"){
                steps {
                cleanWs()
          }
        }


        stage("Checkout from SCM"){
                steps {
                    git branch: 'main', credentialsId: 'github', url: 'https://github.com/sumojon/register-app-gova.git'
         }
        }

     stage("Build Application"){
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
		        withSonarQubeEnv(credentialsId: 'sonartoken') { 
                        sh "mvn sonar:sonar"
		        }
	           }	

           }
           }

stage("Quality Gate"){
           steps {
               script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonartoken'
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
