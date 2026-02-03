pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                sh 'chmod +x gradlew'
                // Generates JaCoCo reports needed for the next stage
                sh './gradlew clean test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                sh "./gradlew sonar \
                    -Dsonar.host.url=http://localhost:9000 \
                    -Dsonar.token=squ_74d4908fceff6bac16b1c89474a7302f1217c011 \
                    -Dsonar.projectKey=bits-ls3-2023vz70077 \
                    -Dsonar.coverage.jacoco.xmlReportPaths=build/reports/jacoco/test/jacocoTestReport.xml"
            }
        }

        stage('Archive Artifact') {
            steps {
                sh './gradlew jar'
                archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint: true
            }
        }
    }
}
