pipeline {
    agent any
    stages {
        stage('Build Artifact - Maven') {
            steps {
                sh "mvn clean package -DskipTests=true"
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
        stage('Unit Tests - JUnit and Jacoco') {
            steps {
                sh "mvn test"
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                    jacoco(execPattern: 'target/jacoco.exec')
                }
            }
        }
        stage('Docker Build and Push') {
            steps {
                script {
                    sh "docker build -t abhijeetsingh1/numeric-app:${env.GIT_COMMIT} ."
                    sh "docker push abhijeetsingh1/numeric-app:${env.GIT_COMMIT}"
                }
            }
        }
    }
}
