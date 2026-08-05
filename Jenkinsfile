pipeline {

    agent any

    tools {
        maven 'maven'
    }

    environment {
        TENANT_ID = "a3bc4ae6-05ef-47f8-8c26-c5ffbe91a1ed"
    }

    stages {

        stage('Check Out from Git') {
            steps {
                git branch: 'prod', url: 'https://github.com/palles959/project4.git'
            }
        }

        stage('Maven Validate') {
            steps {
                sh 'mvn validate'
            }
        }

        stage('Maven Compile') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Maven Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Maven Install') {
            steps {
                sh 'mvn install'
            }
        }

        stage('Trivy Scan') {
            steps {
                echo "Trivy Scan Started"
                sh 'trivy fs --format table --output trivy-report.txt --severity HIGH,CRITICAL .'
                echo "Trivy Scan Finished"
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SCANNER_HOME = tool 'sonar-scanner'
            }
            steps {
                withSonarQubeEnv('sonarserver') {
                    sh """
                        ${SCANNER_HOME}/bin/sonar-scanner \
                        -Dsonar.organization=palles959 \
                        -Dsonar.projectName=project4 \
                        -Dsonar.projectKey=project4 \
                        -Dsonar.java.binaries=target/classes
                    """
                }
            }
        }

        stage('Maven Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Sonar Quality Gate') {
            steps {
                script {
                    timeout(time: 1, unit: 'MINUTES') {
                        waitForQualityGate abortPipeline: true, credentialsId: 'sonar'
                        echo "Sonar Quality Gate Finished"
                    }
                }
            }
        }
    } 
} 
