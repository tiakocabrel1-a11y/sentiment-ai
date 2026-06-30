pipeline {
    agent any

    environment {
        REGISTRY_USER = 'tiakocabrel1-a11y' 
        IMAGE_NAME    = 'sentiment-ai'
    }

    stages {
        stage('Checkout') {
            steps {
                // Récupère le code depuis ton dépôt GitHub
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                echo 'Build et tests de l application...'
            }
        }

        stage('Docker Build & Push') {
            steps {
                echo 'Connexion à la Registry GitHub (GHCR)...'
                // Utilisation du bon ID de credentials 'github-token'
                withCredentials([usernamePassword(credentialsId: 'github-token', usernameVariable: 'GH_USER', passwordVariable: 'GH_TOKEN')]) {
                    sh "echo \$GH_TOKEN | docker login ghcr.io -u \$GH_USER --password-stdin"
                    
                    echo 'Construction de l image Docker...'
                    sh "docker build -t ghcr.io/\${REGISTRY_USER}/\${IMAGE_NAME}:latest ."
                    
                    echo 'Publication de l image sur GitHub Packages...'
                    sh "docker push ghcr.io/\${REGISTRY_USER}/\${IMAGE_NAME}:latest"
                }
            }
        }
    }
    
    post {
        success {
            echo 'Félicitations ! Le pipeline s est terminé avec succès. 🎉'
        }
        failure {
            echo 'Le pipeline a échoué. Regarde les logs ci-dessus pour comprendre pourquoi. ❌'
        }
    }
}