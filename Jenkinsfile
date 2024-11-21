@Library("Library@DevOps") _

pipeline {
    agent {label 'slave-node'}

    stages {
        stage('Checkout code') {
            steps {
                codeCheckout('DevOps', 'https://github.com/iemafzalhassan/Springboot-BankApp.git')
            }
        }
        stage('build') {
            steps {
                buildImage("springboot-application")
            }
        }
        stage('Push Image') {
            steps {
                pushImage("springboot-application")
            }
        }
        stage('Deploy'){
            steps{
                deploy()
            }
        }
        
    }
}
