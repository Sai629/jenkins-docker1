pipeline {
    agent{
        docker{
            image "maven:3.6.3-jdk-8"
        }
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
