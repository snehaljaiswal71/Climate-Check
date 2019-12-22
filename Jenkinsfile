 pipeline {

    options {
        buildDiscarder(logRotator(numToKeepStr: '50', artifactNumToKeepStr: '5'))
    }
    agent any

    tools {
        maven 'maven_3.0.5'
    }

    stages {
        stage('Code Compilation') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
           steps {
                sh 'docker build -t climatecontrol .'
           }
         }

		stage("ECR Login") {
            steps {
                withAWS(credentials:'ecr-credentials') {
                    script {
                        def login = ecrLogin()
                        sh "${login}"
                    }
                }
            }
        }

        stage('Upload Docker Image to AWS ECR') {
            steps {
                sh 'docker push 340866772701.dkr.ecr.ap-south-1.amazonaws.com/climatecontrol:latest'
            }
        }

        stage('Deploy to Production') {
            steps {
                sh 'date'
            }
        }

    }
}