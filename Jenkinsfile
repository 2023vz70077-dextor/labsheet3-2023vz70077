pipeline {

    agent any



    stages {

        stage('Checkout') {

            steps {

                // Pulls the code from the GitHub repository configured in the Jenkins job

                checkout scm

            }

        }



        stage('Build & Test') {

            steps {

                // Ensure the Gradle wrapper is executable in the Unix environment

                sh 'chmod +x gradlew'

                // Executes clean and test to generate JaCoCo reports

                sh './gradlew clean test'

            }

        }



        stage('SonarQube Analysis') {

    steps {

        withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {

            sh "./gradlew sonar \

                -Dsonar.host.url=http://localhost:9000 \

                -Dsonar.token=${SONAR_TOKEN} \

                -Dsonar.projectKey=bits-ls3-2023vz70077 \

                -Dsonar.coverage.jacoco.xmlReportPaths=build/reports/jacoco/test/jacocoTestReport.xml"

        }

    }

}



        stage('Archive Artifact') {

            steps {

                // Generates the JAR file

                sh './gradlew jar'

                // Archives the JAR for download from the Jenkins UI

                archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint: true

            }

        }

    }

}
