pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    stages {

        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=temp'
                }
            }
        }
    }
}
