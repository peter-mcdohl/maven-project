pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '/usr/local/apache-tomcat-8.5.35', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '/usr/local/apache-tomcat-8.5.35-prod', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

    tools {
        maven 'localMaven'
        jdk 'localJDK'
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
                        sh "cp **/target/*.war ${params.tomcat_dev}/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "cp **/target/*.war ${params.tomcat_prod}/webapps"
                    }
                }
            }
        }
    }
}