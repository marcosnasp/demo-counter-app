pipeline {
    agent any 
    tools {
       maven 'maven'
    }
    stages {
        stage ('Git Checkout') {
          steps {
             script {
                 
                    git branch: 'main', url: 'https://github.com/marcosnasp/demo-counter-app.git'
                }
            }
        }
        stage ('UNIT testing') {
            steps {
                withMaven {
                    sh 'mvn test'
                }
            }
        }
        stage ('Integration testing') {
            steps {
                withMaven {
                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
        stage ('Maven build') {
            steps {                
                withMaven {
                    sh 'mvn clean install'
                }
            }
        }
        stage ('Static code analysis') {
            steps {
                script {
                    withSonarQubeEnv (credentialsId: 'sonar-api') {
                        sh 'mvn clean package sonar:sonar'
                    }
                } 
            }
         }
         stage ('Quality Gate Status') {    
             steps {
                    script {                        
                        waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
                    }
                }
            }
        }
}
