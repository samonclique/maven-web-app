pipeline {
    agent any

    tools {
        maven "maven3.9.6"
    }

    stages {
        stage("Git Clone") {
            steps {
                git 'https://github.com/yormengh/maven-web-app.git'
            }
        }

        stage("Build with Maven") {
            steps {
                sh "mvn clean"
                echo "cleaning done"
            }
        }

        stage("Test with Maven") {
            steps {
                sh "mvn test"
                echo "testing done"
            }
        }

        stage("package with Maven") {
            steps {
                sh "mvn package"
                echo "packaging done"
            }
        }

        stage('SonarQube Analysis') {
            environment {
                ScannerHome = tool 'sonar5.0'
            }
            steps {
                script {
                    withSonarQubeEnv('sonarqube') {
                        sh "${ScannerHome}/bin/sonar-scanner -Dsonar.projectKey=yormen"
                    }
                }
            }
        }

        stage("Upload Artifact to Nexus") {
            steps {
               nexusArtifactUploader artifacts: [[artifactId: '01-maven-web-app', classifier: '', file: '/var/lib/jenkins/workspace/Webapp-Declarative-Pipeline/target/maven-web-app.war', type: 'war']], credentialsId: 'nexus-id', groupId: 'in.ashokit', nexusUrl: '18.116.13.148:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'webapp-release', version: '3.0-RELEASE' 
                echo "Uploading to Nexus done"
            }
        }

        stage("Deploy to Tomcat") {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat-access', path: '', url: 'http://3.140.253.235:8080')], contextPath: null, war: 'target/*.war'
                echo "Deployment to Tomcat done"
            }
        }
    }
}
