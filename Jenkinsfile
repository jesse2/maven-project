pipeline {
    agent any

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deploy to Staging'){
            steps {
                sh "cp **/target/*.war /opt/tomcat/webapps"
            }
            post {
                success {
                    echo 'Code deployed to Stage.'
                }

                failure {
                    echo ' Deployment failed.'
                }
            }
        }

        stage ('Deploy to Production'){
            steps {
                timeout(time:5, unit:'DAYS'){
                    input message:'Approve PRODUCTION Deployment?'
                    }
                sh "cp **/target/*.war /opt/tomcat2/webapps"
            }
            post {
                success {
                    echo 'Code deployed to Production.'
                }

                failure {
                    echo ' Deployment failed.'
                }
            }
        }
    }
}
