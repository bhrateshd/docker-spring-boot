pipeline {
    agent any

    environment {
        registry = "https://hub.docker.com/repositories/bhrateshd/docker-spring-boot"
        registryCredential = 'bhrateshd'

    }
    stages {
        stage('Checkout'){
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/bhrateshd/docker-spring-boot.git']]')
            }
        
        }
        stage('Build JAR') {
            steps{
                sh 'mvn clean install'
            }
        }
        stage('Build Image') {
            steps{
                sh 'docker build -t docker-spring-boot:latest .'
            }
        }        
        stage('Push Image') {
            steps{
                withDockerRegistry([credentialsId: registryCredential, url: registry]) {
                sh 'docker push docker-spring-boot:latest'
                }
            }
        }
        stage('Helm Package') {
            steps{
                sh 'helm package springboot'
            }
        }
         stage('Helm install') 
         {
             steps{
                 sh 'helm upgrade myrelease springboot-0.1.0.tgz'
             }
        }
    }
}