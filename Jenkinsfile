pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Build & Test') {
            steps { sh 'gradle clean test' }
        }
        stage('SonarQube Analysis') {
            steps {
                // Ensure SonarQube is configured in Jenkins System Manage
                withSonarQubeEnv('SonarQube') { 
                    sh 'gradle sonar'
                }
            }
        }
        stage('Archive') {
            steps {
                sh 'gradle jar'
                archiveArtifacts artifacts: 'build/libs/*.jar', allowEmptyArchive: false
            }
        }
    }
}
