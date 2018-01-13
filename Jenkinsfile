pipeline {
    agent any
    
    parameters { 
         string(name: 'aws_tomcat_dev', defaultValue: '18.217.172.142', description: 'Staging Server')
         string(name: 'aws_tomcat_prod', defaultValue: '18.217.192.115', description: 'Production Server')
    } 

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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i C:/devtools/aws/tomcat-demo-key-pair/tomcat-demo.pem **/target/*.war ec2-user@${params.aws_tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i C:/devtools/aws/tomcat-demo-key-pair/tomcat-demo.pem **/target/*.war ec2-user@${params.aws_tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}