pipeline {
     agent any;
     tools {
            maven 'maven 3'
            jdk 'jdk8'
    }
    
    stages {
        stage('git'){
            steps {
                sh 'echo checkout code'
                checkout scm
            }
            
        }
        stage('compile'){
            steps {
               sh 'mvn clean compile'   
            }
        }
          stage('test'){
            steps {
                 sh 'mvn test' 
            } 
        }
        stage('junit'){
            steps {
                junit '**/*.xml'
            }
            
        }
        stage('sonar'){
            steps {
                withSonarQubeEnv('sonar') {
                  sh 'mvn sonar:sonar'
                }
            }
         }
    }
}
