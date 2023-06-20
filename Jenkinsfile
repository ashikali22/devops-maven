pipeline {
    agent any
    tools {
        maven 'maven3'
    }
    stages {
        stage('Build Maven') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ashikali22/devops-maven.git']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image') {
            steps {
                script {
                    sh 'docker build -t ashikali22/devops-integration .'
                }
            }
        }
        stage('Push image to Hub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
                        sh 'docker login -u ashikali22 -p ${dockerhubpwd}'
                    }
                    sh 'docker push ashikali22/devops-integration'
                }
            }
        }
    }
}
