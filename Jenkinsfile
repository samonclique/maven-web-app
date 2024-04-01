pipeline {
    agent any
    tools {
        maven "maven3"
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
    }
}