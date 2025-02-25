pipeline {
    agent any

    environment {
        SONAR_URL = 'http://localhost:9000'
        SONAR_TOKEN = credentials('squ_4988886d9bcf62ae3aa087138e6c0c82e3466349')  // Replace with your credential ID
        TOMCAT_PATH = '/opt/tomcat/webapps/'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: 'rupanshi01', 
                    url: 'https://github.com/rupanshi01/devopsproj.git',
                    branch: 'main'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=myapp -Dsonar.host.url=$SONAR_URL -Dsonar.login=$SONAR_TOKEN'
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh 'cp target/myapp.war $TOMCAT_PATH'
                sh '/opt/tomcat/bin/shutdown.sh'
                sh '/opt/tomcat/bin/startup.sh'
            }
        }
    }
}