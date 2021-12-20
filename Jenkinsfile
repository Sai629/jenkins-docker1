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
        stage('package application'){
            steps {
                sh 'mvn package -D skiptest' 
            }    
        }
        stage('add to repo'){
            steps {
                rtMavenResolver (
                    id: 'resolver1',
                    serverId: 'jenkins-artifactory',
                    releaseRepo: 'libs-release-local',
                    snapshotRepo: 'libs-snapshot-local'
                )  
                rtMavenDeployer (
                    id: 'deployer1',
                     serverId: 'jenkins-artifactory',
                    releaseRepo: 'libs-release-local',
                    snapshotRepo: 'libs-snapshot-local'
                )
                rtMavenRun (
                    tool:'maven 3',
                    pom: 'pom.xml',
                    goals: 'install',
                    resolverId: 'resolver1',
                    deployerId: 'deployer1',
                )
            }    
        }
    }
}
