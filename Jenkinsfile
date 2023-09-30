pipeline {
    agent any
    
    tools {
        jdk "jdk17"
        maven "maven"
    }
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }
    
stages {
    stage('Git Checkout'){
        steps {
            git branch: 'main', url: 'https://github.com/ToluOlorode/SpringBoot-WebApplication.git'
        }
    }
    
    stage('Compile') {
     steps {
        sh "mvn compile"
     }
    }
    
    stage('Test'){
        steps {
            sh "mvn test"
        }
    }

    
    stage('SonarQube Analysis'){
        steps {
           withSonarQubeEnv('sonar') {
            sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Springboot \
            -Dsonar.java.binaries=. \
            -Dsonar.projectKey=Springboot '''
        }
        }
    }
    
    stage('Build'){
        steps {
           sh "mvn clean package"
        }
        
    }
    
    stage('Build and Push Docker'){
        steps {
            script{
            withDockerRegistry(credentialsId: 'dockerhub', toolName: 'docker') }
{
            sh "docker build -t springboot ."
            sh "docker tag springboot danityprojects/springboot"
            sh "docker push danityprojects/springboot"
        
    }
   }
  }
    stage('Deploy to Docker') {
        steps {
            sh "docker run -d -p 8085:8085 danityprojects/springboot"}
    }
 }
}
