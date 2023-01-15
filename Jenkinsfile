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
    }
    
    }