pipeline {
    agent any
    
    tools{
        maven 'localMaven'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
           }
            post{
                success{
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                    echo '!!!Done!!!'
                    
             }
          }
        }
        stage('Deploy to Staging'){
            steps{  
                build job: 'deploy-to-staging'
            }
        }
        stage('Deploy to Production'){
            steps{
                timeout(time:5, unit:'DAYS'){
                input message:'Approve production deployment?'
                }
                build job: 'deploy-to-prod'
            }
        
        post{
            success{
                echo 'Successfully deployed to prod'
            }
            failure{
                echo 'Not deployed to prod'
           }
        }
       }
    }
}
  
