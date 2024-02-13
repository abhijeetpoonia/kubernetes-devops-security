pipeline {
    agent any

    stages {
        stage('Build Artifact') {
            steps {
                sh "mvn clean package -DskipTests=true"
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
        stage('unit-Test') { // Corrected stage name
            steps {
                sh "mvn test"
            }
        }   
    }
}
