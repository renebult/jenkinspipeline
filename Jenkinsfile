pipeline {
    agent any
    
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

        stage('Deploy to Staging') {
            steps {
                build job: 'deploy-to-staging'
            }
        }

        stage('Deploy to Production') {
            steps {
                timeout(time:5, unit:'DAYS') {
                    input message: 'Approve Production Deployment?'
                }
                build job: 'deploy-to-prd'
            }
            post {
                success {
                    echo 'Code deployed to production successfull.'
                }
                failure {
                    echo 'Deployment to production failed.'
                }
            }
        }
    }
}