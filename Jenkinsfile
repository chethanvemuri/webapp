pipeline {
    agent any
parameters { 
         
    string(name: 'tomcat_prod', defaultValue: '18.218.209.248', description: 'Production Server')
    } 

    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }
     
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package' 
                }
                post {
                  success
                  {
                    echo 'Now Archiving...'
                          archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deployments'){
            parallel{
        
          stage ("Deploy to Production"){
                    steps {
                        sh "scp -i  /var/lib/jenkins/cheth.pem ./target/*.war ec2-user@${params.tomcat_prod}:/opt/apache-tomcat-7.0.53/webapps"
                    }
              }
         }
    }
 }
}
