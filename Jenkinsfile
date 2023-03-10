pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    stages {
        stage('build') {
            steps {
                sh 'mvn clean install package'
            }                    
            }

        stage('upload artifact') {
            steps {
rtUpload (
    serverId: 'artifactory',
    spec: '''{
          "files": [
            {
              "pattern": "./target/restro.war",
              "target": "generic-local"
            }
         ]
    }''',
)
 
                }
        }

        stage('download artifact')   {
            
        steps{

        rtDownload (
    serverId: 'artifactory',
    spec: '''{
          "files": [
            {
              "pattern": "gerenic-local/restro.war",
              "target": "./restro.war"
            }
          ]
    }'''
        )  
                        
            }
        
    }

    stage("deploy war file"){
        steps{
                            withCredentials([usernamePassword(credentialsId: 'tomcat_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {

                                script{
                        #sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@104.42.42.112 \"rm -rf /home/cloud_user/apps/tomcat/webapps/restro*\""
                        
                        sh "sshpass -p '$USERPASS' scp -o StrictHostKeychecking=no target/restro.war $USERNAME@104.42.42.112:/home/cloud_user/apps/tomcat/webapps"
                                }
                                }
        }
    }
    }
}

