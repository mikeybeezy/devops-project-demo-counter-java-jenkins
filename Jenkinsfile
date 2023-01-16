pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', credentialsId: 'github_credentials', url: 'git@github.com:mikeybeezy/devops-project-demo-counter-java-jenkins.git'
            }
        }
        }
        stage('UNIT testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn test'
                }
            }
            
            
        }
        stage('Intergration Testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
        stage('Maven Build'){
            
            steps{
                
                sh 'mvn clean install '
            }
        }
        stage('Static Code Analysis'){
            
            steps{
                
                script{
                    
                    withSonarQubeEnv(credentialsId: 'sonar_jenkins') {
                        
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }
        stage('Quality Gate Status'){
            
            steps{
                
                script{
                    
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar_jenkins'
                }
            }
        }
        stage('Push to Artifact Repository'){

            steps{

                script{

                    def readPomvVersion = readMavenPom file: 'pom.xml'

                    nexusArtifactUploader artifacts: [[artifactId: 'springboot', 
                    classifier: '', 
                    file: 'target/Uber.jar',
                    type: 'jar']], 
                    credentialsId: 'nexus-access-details',
                    groupId: 'com.example', 
                    nexusUrl: '34.251.22.225:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'springboot_devops_project/', 
                    version: "${readPomvVersion}"
                    }
                }
         }
         stage('docker image build'){

            steps{

                script{

                    sh 'docker build -t $JOB_NAME:v1.$BUILD_ID .'

                    sh 'docker tag $JOB_NAME:v1.$BUILD_ID mikeybabs/$JOB_NAME:v1.$BUILD_ID'

                    sh 'docker tag $JOB_NAME:v1.$BUILD_ID mikeybabs/$JOB_NAME:latest'


                }
            }
        } 
        stage('push to Docker Hub') {

            steps{

                script{

                    withCredentials([string(credentialsId: 'DockerHubPassword', variable: 'DockerHub_Pass')]) {

                        sh 'docker login -u mikeybabs -p ${DockerHub_Pass}'

                        sh 'docker push $JOB_NAME:v1.$BUILD_ID mikeybabs/$JOB_NAME:v1.$BUILD_ID'

                        sh 'docker push $JOB_NAME:v1.$BUILD_ID mikeybabs/$JOB_NAME:v1.latest'

                    }


                }
            }





        }
    }

}