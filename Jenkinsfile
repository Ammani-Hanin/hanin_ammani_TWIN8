pipeline {
    agent any

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
                sh 'docker build -t username/springboot-mysql:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                echo 'Push de l’image Docker vers Docker Hub...'
                sh 'docker push username/springboot-mysql:latest'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo 'Déploiement sur Kubernetes...'
                sh 'kubectl apply -f k8s/ -n devops'
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
