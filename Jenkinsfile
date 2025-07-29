pipeline{
    agent{
        label "jenkins-agent"
    }
    tools{
        jdk 'java21'
        maven 'Maven3'
    }
    stages{
        stage("Cleanup Workspace"){
            steps{
                cleanWs()
            }

        }
        stage("Checkout from SCM"){
            steps{
                git branch: 'main', url: 'https://github.com/YuzheZhang0/complete-prodcution-e2e-pipeline'
            }
        }
        stage("Build Application"){
            steps{
                sh "mvn clean package"
            }
            
        }
        stage("Test Application"){
            steps{
                sh "mvn test"
            }
        
        }
        stage("Sonarqube Analysis"){
            steps{
                withSonarQubeEnv (installationName: 'sonarqube-scannner', credentialsId: 'jenkins-sonarqube-token'){
                    sh "mvn sonar:sonar"
                }
            }
        
        }
        stage("Quality Gate"){
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
                    }
                
                }
            }
        
        }
}