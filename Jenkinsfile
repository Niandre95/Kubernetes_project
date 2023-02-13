pipeline {
    agent any
    tools{
        maven 'M2_HOME'
        jdk 'JAVA_HOME'
    }
    environment {
        registry = '868016059835.dkr.ecr.us-east-1.amazonaws.com/geolocation_ecr_rep'
        registryCredential = 'jenkins-ecr'
        dockerimage = ''
    }
    stages {
        stage('Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/Niandre95/Kubernetes_project.git'
            }
        }
        stage('Code Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
	stage ('CHECKSTYLE'){
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
	}
        stage('Build Image') {
            steps {
                script{
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                } 
            }
        }
        stage('Deploy image') {
            steps{
                script{ 
                    docker.withRegistry("https://"+registry,"ecr:us-east-1:"+registryCredential) {
                		dockerImage.push('latest')
                    }
                }
            }
        }
    }
}

