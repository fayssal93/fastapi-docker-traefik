pipeline {
    agent any

    environment {
        DOCKER_ID = "bfaycal2020"
        DOCKER_IMAGE = "fastapi-app" // Nom de l'image Docker que vous souhaitez utiliser
        DOCKER_TAG = "v.${BUILD_ID}.0"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Étape de construction de l'image Docker
                    sh '''
                    docker rm -f jenkins
                    docker build -t $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG .
                    sleep 6
                    '''
                }
            }
        }

        stage('Start FastAPI Application') {
            steps {
                script {
                    // Étape pour démarrer le conteneur FastAPI à l'aide de docker-compose
                    sh "docker-compose up -d"
                }
            }
        }

        stage('Test FastAPI Application') {
            steps {
                script {
                    // Ajoutez ici vos étapes de test de l'application FastAPI si nécessaire
                    // Par exemple, vous pouvez utiliser des commandes curl pour effectuer des tests d'acceptation
                    sh "curl http://localhost:8000" // Exemple de commande de test
                }
            }
        }

        stage('Stop FastAPI Application') {
            steps {
                script {
                    // Étape pour arrêter le conteneur FastAPI
                    sh "docker-compose down"
                }
            }
        }

        stage('Push Docker Image to Registry') {
            environment {
                DOCKER_PASS = credentials("DOCKER_HUB_PASS") // Nous récupérons le mot de passe Docker depuis le texte secret appelé docker_hub_pass enregistré sur Jenkins
            }

            steps {
                script {
                    sh '''
                    docker login -u $DOCKER_ID -p $DOCKER_PASS
                    docker push $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG
                    '''
                }
            }
        }
    }
}
