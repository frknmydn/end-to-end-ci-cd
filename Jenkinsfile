pipeline{
    agent any

    stages{
        stage('Git checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/frknmydn/end'
            }
        }

        stage('Unit testing'){
            steps{
                bat 'mvn test'
            }
        }

        stage('Integration test'){
            steps{
                bat 'mvn verify -DskipunitTests'
            }
        }
        stage('Maven build'){
            steps{
                bat 'mvn clean install'
            }
        }
        stage('static code analysis'){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-api') {
                        bat 'mvn clean package sonar:sonar'
                }
                
                }
            }
        }
        
        stage('Quality gate status'){
            steps{
                sleep(20)
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
                }
            }
        }
        stage('upload war file to nexus3'){
            steps{
                script{
                    def readPomVersion = readMavenPo file: 'pom.xml'
                    nexusArtifactUploader artifacts: 
                    [
                        [
                            artifactId: 'springboot', classifier: '', file: 'target/Uber.jar', type: 'jar'
                        ]
                    ], 
                    credentialsId: 'nexus-auth', 
                    groupId: 'com.example', 
                    nexusUrl: 'localhost:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http',
                    repository: 'demoapp-release', 
                    version: "${readPomVersion}"
                }
            }
        }
    }
}
