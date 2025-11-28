pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Récupération du code source...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Compilation du projet...'
                sh 'mvn clean install'
            }
        }

        stage('Tests') {
            steps {
                echo 'Exécution des tests...'
                sh 'mvn test'
            }
        }

    }

    post {
        success {
            echo "🎉 Build réussi !"
        }
        failure {
            echo "❌ Build échoué !"
        }
    }
}
