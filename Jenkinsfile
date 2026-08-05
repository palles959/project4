pipeline {

    agent any

    tools {

        maven 'maven'

    }

    environment {
        TENANT_ID="a3bc4ae6-05ef-47f8-8c26-c5ffbe91a1ed"
    }

    stages {
        stage('Check Out from Git') 
        {
            steps {
                git branch: 'prod' , url: 'https://github.com/palles959/project4.git'
            }
        }

        stage('Maven Validate') 
        {
            steps {
                sh 'mvn validate'
            }
        }

        stage('Maven Compile') 
        {
            steps {
                sh 'mvn compile'
            }
        }
        stage('Maven Test') 
        {
            steps {
                sh 'mvn test'
            }
        }
        stage('Maven Install') 
        {
            steps {
                sh 'mvn install'
            }
        }
        stage(' Trivy Scan')
        {
            steps {
                echo "Trivy Scan Started"
                sh 'trivy fs --format table --output trivy-report.txt --severity HIGH,CRITICAL .'
                echo "Trivy Scan Finished"
            }
        }
    }
}
