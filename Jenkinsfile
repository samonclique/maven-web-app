pipeline {
    agent any
    tools {
        maven "maven3"
    }
    environment {
        scannerhome = tool "sonar5"
    }
    stages {
        stage ("Git clone") {
            steps{
                git branch: 'samuel', url: 'https://github.com/samonclique/maven-web-app.git'
            }
        }
        stage("Build and Test") {
            steps {
                sh '''
                mvn clean
                mvn test
                mvn package
                '''
            }
        }
        stage ("Sonaqube Analysis") {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'jenkins-sonar-key') {
                       sh "${scannerhome}/bin/sonar-scanner -Dsonar.projectKey=samonclique -Dsonar.projectName=Yormen"
                    }
                }
            }
        }
    }
}