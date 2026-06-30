pipeline {
    agent any

    environment {
        // Remplace par ton identifiant GitHub si besoin, mais tiakocabrel1-a11y semble être le bon
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
                // Ici, tu peux ajouter tes commandes de test si tu en as (ex: pytest)
            }
        }

        stage('Docker Build & Push') {
            steps {
                echo 'Connexion à la Registry GitHub (GHCR)...'
                // Utilise le token enregistré dans Jenkins pour se connecter à GitHub Packages
                withCredentials([usernamePassword(credentialsId: 'tiakocabrel1-a11y', usernameVariable: 'GH_USER', passwordVariable: 'GH_TOKEN')]) {
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