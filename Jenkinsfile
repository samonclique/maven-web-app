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
        stage ("Deploy to Nexus") {
            steps {
                withMaven(globalMavenSettingsConfig: 'maven-project', jdk: '', maven: 'maven3', mavenSettingsConfig: '', traceability: true) {
                    sh "mvn deploy"
                }
            }
        }
        stage ("Deploy to tomcat") {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'jenkins-tomcat-ID', path: '', url: 'http://3.16.159.98:8080/')], contextPath: null, war: 'target/*.war'
            }
        }
    }
}