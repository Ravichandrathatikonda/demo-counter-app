pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', url: 'https://github.com/Ravichandrathatikonda/demo-counter-app.git'
                }
            }
        }
        stage('UNIT testing'){
            
            steps{
                
                script{
                    
                    bat 'mvn test'
                }
            }
        }
        stage('Integration testing'){
            
            steps{
                
                script{
                    
                    bat 'mvn verify -DskipUnitTests'
                }
            }
        }
        stage('Maven build'){
            
            steps{
                
                script{
                    
                    bat 'mvn clean install'
                }
            }
        }
        stage('Static code analysis'){
            
            steps{
                
                script{
                    
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        
                        bat 'mvn clean package sonar:sonar \
  -Dsonar.projectKey=com.example:springboot \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=8fab3ebf976b8cc8d20cdebf87cd6e81c5b7cfca'
                    }
                }
                    
            }
        }
        /* stage('Quality Gate Status'){
                
            steps{
                    
                script{
                        
                        waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                    }
                }
        } */
        stage('Upload Jar to Nexus Repo'){
                
            steps{
                
                script{
                    def readPomVersion = readMavenPom file: 'pom.xml'
                    def nexusRepo = readPomVersion.version.endsWith("SNAPSHOT") ? "DevOpsProject1-snapshot" : "DevOpsProject1-release"
                    nexusArtifactUploader artifacts: [[artifactId: 'springboot', classifier: '', file: 'target/Uber.jar', type: 'jar']], credentialsId: 'nexus-auth', groupId: 'com.example', nexusUrl: 'localhost:8081', nexusVersion: 'nexus3', protocol: 'http', repository: nexusRepo, version: "${readPomVersion.version}"
                }
            }
        }
        stage('Docker Image Build'){
            
            steps{
                
                script{
                     bat 'docker build -t $JOB_NAME:v1.$BUILD_ID .'
                     bat 'docker image tag $JOB_NAME:v1.$BUILD_ID ravichandra0702/$JOB_NAME:v1.$BUILD_ID'
                     bat 'docker image tag $JOB_NAME:v1.$BUILD_ID ravichandra0702/$JOB_NAME:latest'
                }
            }
        }
            
    }       
        
}
