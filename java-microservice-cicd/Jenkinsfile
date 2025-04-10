pipeline {
    agent any

    environment {
        REGISTRY = "docker.io/srikar2610/my-app"
        IMAGE_TAG = "${GIT_COMMIT}"
        SONARQUBE_SERVER = 'SonarQubeServer'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean package'
            }
        }

        
        stage('Docker Build & Push') {
    when {
        branch 'develop'
    }
    steps {
        script {
            dockerImage = docker.build("srikar2610/java-microservice:${env.BUILD_NUMBER}")
            docker.withRegistry('https://index.docker.io/v1/', 'docker-hub') {
                dockerImage.push()
            }
        }
    }
}


        stage('Kubernetes Deploy') {
            when {
                branch 'main'
            }
            steps {
                sh """
                kubectl apply -f k8s/deployment.yaml
                kubectl apply -f k8s/service.yaml
                """
            }
        }
    }
}
