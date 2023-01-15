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

                    nexusArtifactUploader artifacts: [[artifactId: 'springboot', classifier: '', file: 'target/springboot/1.0.0.jar', type: 'jar']], credentialsId: 'nexus-access-details', groupId: 'com.example', nexusUrl: '34.251.22.225:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'http://34.251.22.225:8081/repository/springboot_devops_project/', version: '1.0.0'
            }
        }
    }
    
    }