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
                    sh 'mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=sai629_jenkins-docker1'
                }
            }
        }
        stage('package application'){
            steps {
                sh 'mvn package -D skiptest' 
            }    
        }
         stage('Generic Artificatory') {
                steps {
                rtUpload (
                        serverId: 'art1',
                        spec: '''{
                            "files": [
                                {
                                "pattern": "target/ci-jenkins-docker-0.0.1.jar",
                                "target": "libs-snapshot-local/jenkins-docker1/${BUILD_NUMBER}"
                                },
                                {
                                "pattern": "pom.xml",
                                "target": "libs-snapshot-local/jenkins-docker1/${BUILD_NUMBER}"
                                }
                            ]
                        }''',
                    
                        // Optional - Associate the uploaded files with the following custom build name and build number,
                        // as build artifacts.
                        // If not set, the files will be associated with the default build name and build number (i.e the
                        // the Jenkins job name and number).
                        buildName: "${JOB_NAME}",
                        buildNumber: "${BUILD_NUMBER}"
                 )
                }
            }
            stage('Promote Build') {
                steps {
                    withCredentials([usernamePassword(credentialsId: 'artifactory', passwordVariable: 'PASS', usernameVariable: 'USER')])  {
                     sh 'curl -u${logindata} -X PUT "http://192.168.50.100:8081/artifactory/api/storage/libs-snapshot-local/jenkins-docker1/${BUILD_NUMBER}/ci-jenkins-docker-0.0.1.jar?properties=Promoted=Yes"'
                 }
              }
            }
//         stage('add to repo'){
//             steps {
//                 rtMavenResolver (
//                     id: 'resolver1',
//                     serverId: 'art1',
//                     releaseRepo: 'libs-release-local',
//                     snapshotRepo: 'libs-snapshot-local'
//                 )  
//                 rtMavenDeployer (
//                     id: 'deployer1',
//                      serverId: 'art1',
//                     releaseRepo: 'libs-release-local',
//                     snapshotRepo: 'libs-snapshot-local'
//                 )
//                 rtMavenRun (
//                     tool:'maven 3',
//                     pom: 'pom.xml',
//                     goals: 'install',
//                     resolverId: 'resolver1',
//                     deployerId: 'deployer1',
//                 )
//             }    
//         }
    }
}
