pipeline {
    agent { label 'Jenkins-Agent' }
    tools {
        jdk 'java17'
        maven 'maven3'
    }

  stages{
        stage("Cleanup Workspace"){
                steps {
                cleanWs()
          }
        }


        stage("Checkout from SCM"){
                steps {
                    git branch: 'main', credentialsId: 'github', url: 'https://github.com/sumojon/feb1st2024.git'
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
  }
  }
