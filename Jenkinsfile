pipeline {
    agent any

    tools {
        maven 'maven3'
    }

    stages {

       /* stage('Checkout') {
            steps {
                git branch: 'develop', url: 'https://github.com/Herve-D/Team1-group.git'
            }
        }*/

        stage('Build Backend') {
            steps {
                dir('demo-app') {
                    sh "mvn clean install -DskipTests"
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    dir('demo-backend') {
                        sh '''
                            mvn clean verify sonar:sonar                               -Dsonar.projectKey=demo-backend                               -Dsonar.host.url=$SONAR_HOST_URL                               -DskipTests
                        '''
                    }
                }
            }
        }

        stage("Quality Gate") {
            steps {
                script {
                    timeout(time: 3, unit: 'MINUTES') {
                        waitForQualityGate abortPipeline: true
                    }
                }
            }
        }
    }
}