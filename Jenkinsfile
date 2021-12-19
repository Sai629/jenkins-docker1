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
                sh 'mvn package' 
            }    
        }
        stage('add to repo'){
            steps {
                rtMavenResolver (
                    id: 'resolver1',
                    serverId: 'jenkins-artifactory',
                    releaseRepo: 'saikiran-libs-release-local',
                    snapshotRepo: 'saikiran-libs-snapshot-local'
                )  
                rtMavenDeployer (
                    id: 'deployer1',
                     serverId: 'jenkins-artifactory',
                    releaseRepo: 'saikiran-libs-release-local',
                    snapshotRepo: 'saikiran-libs-snapshot-local'
                    properties: ['Branch=${BRANCH_NAME}', 'key2=value2']
                )
                rtMavenRun (
                    // Tool name from Jenkins configuration.
                    // Set to true if you'd like the build to use the Maven Wrapper.
                    useWrapper: true,
                    pom: 'maven-example/pom.xml',
                    goals: 'install',
                    // Maven options.
                    opts: '-Xms1024m -Xmx4096m',
                    resolverId: 'resolver1',
                    deployerId: 'deployer1',
                    // If the build name and build number are not set here, the current job name and number will be used:
                    buildName: '${JOB_BASE_NAME}',
                    buildNumber: '${BUILD_NUMBER}',
                    // Optional - Only if this build is associated with a project in Artifactory, set the project key as follows.
                    project: 'my-project-key'
                )
            }    
        }
    }
}
