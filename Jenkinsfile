@Library('my-lib') _  

pipeline {
    agent { label 'build-agent' }

    stages {
        stage('Run Unit Tests') {
            steps { runUnitTest() }
        }
        stage('Build Application') {
            steps { buildApp() }
        }
        stage('Build Docker Image') {
            steps { buildImage() }
        }
        stage('Scan Docker Image') {
            steps { scanImage() }
        }
        stage('Push Docker Image') {
            steps { pushImage() }
        }
        stage('Remove Local Image') {
            steps { removeImageLocally() }
        }
        stage('Deploy on K8s') {
            steps { deployOnK8s() }
        }
    }
}
