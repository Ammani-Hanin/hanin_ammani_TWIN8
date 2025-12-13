pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "username/springboot-mysql:latest"
        DOCKERFILE_PATH = "docker/Dockerfile" // chemin vers le Dockerfile
        K8S_MANIFEST_PATH = "k8s/"           // chemin vers les manifests Kubernetes
        DOCKERHUB_CREDENTIALS = "dockerhub-credentials-id" // ID des credentials Jenkins pour Docker Hub
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Récupération du code source...'
                checkout scm
            }
        }

        stage('Build Maven') {
            steps {
                echo 'Build du projet Spring Boot...'
                sh 'mvn clean install -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Construction de l’image Docker...'
                sh "docker build -t ${DOCKER_IMAGE} -f ${DOCKERFILE_PATH} ."
            }
        }

        stage('Push Docker Image') {
            steps {
                echo 'Push de l’image Docker vers Docker Hub...'
                withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CREDENTIALS}", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh "echo $PASSWORD | docker login -u $USERNAME --password-stdin"
                    sh "docker push ${DOCKER_IMAGE}"
                    sh "docker logout"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo 'Déploiement sur Kubernetes...'
                sh "kubectl apply -f ${K8S_MANIFEST_PATH} -n devops"
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline exécuté avec succès'
        }
        failure {
            echo '❌ Erreur dans le pipeline'
        }
    }
}
