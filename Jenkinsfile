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
                    def dockerImage = "abhijeetsingh1/numeric-app:${env.GIT_COMMIT}"
                    withDockerRegistry([credentialsId: "dockerhub", url: "https://index.docker.io/v1/"]) {
                        sh "docker build -t $dockerImage ."
                        sh "docker push $dockerImage"
                    }
                }
            }
        }
        stage('Kubernetes Deployment') {
            steps {
                withKubeConfig([credentialsId: 'kubeconfig']) {
                    sh """
                        sed -i 's#replace#abhijeetsingh1/numeric-app:${env.GIT_COMMIT}#g' k8s_deployment_service.yaml
                        kubectl apply -f k8s_deployment_service.yaml
                    """
                }
            }
        }
    }
}
