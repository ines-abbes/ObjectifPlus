pipeline {
    /* On dit à Jenkins d'exécuter le pipeline sur n'importe quel agent.
       ⚠ Cet agent doit avoir Docker installé et le droit d'exécuter "docker build / docker push". */
    agent any

    /* Variables globales */
    environment {
        // Nom local de l'image 
        backendimage = "objectif_backend_img"
        frontendimage = "objectif_frontend_img"
        
        // Repo folders
        
        backendF = "objectif_backend"
        frontendF = "objectif_frontend"

        // URL du repo GitHub
        GIT_REPO = "https://github.com/ines-abbes/ObjectifPlus.git"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "==> Récupération du code source depuis GitHub"

                /* Clone le repo avec tes identifiants Jenkins ''
                   - branch: mets 'main' ou 'master' selon ta branche */
                git branch: 'master',
                    credentialsId: 'github-cred',
                    url: "${GIT_REPO}"
            }
        }

        stage('Build Docker Image objectifplus -- backend') {
            steps {
                echo "==> Build de l'image Docker locale"

                /* On construit l'image Docker en tag 'latest'
                   Le Dockerfile doit être à la racine du repo */
                sh """
                    docker build -t ${backendimage}:latest ${backendF}
                """
            }
        }

        stage('Push Docker Image objectifplus -- backend') {
            steps {
                echo "==> Push de l'image sur Docker Hub (tag: latest)"

                /* On utilise les identifiants Jenkins ''
                   ATTENTION :
                   - usernameVariable = ton username Docker Hub
                   - passwordVariable = le mot de passe / token Docker Hub
                */
                script {
                    withCredentials([usernamePassword(
                        credentialsId: 'dockerhub-cred',
                        usernameVariable: 'DOCKERHUB_USER',
                        passwordVariable: 'DOCKERHUB_PASS'
                    )]) {

                        // login Docker Hub
                        sh """
                            echo "${DOCKERHUB_PASS}" | docker login -u "${DOCKERHUB_USER}" --password-stdin
                        """

                        // retag l'image locale avec ton namespace Docker Hub
                        sh """
                            docker tag ${backendimage}:latest ${DOCKERHUB_USER}/${backendimage}:latest
                            docker push ${DOCKERHUB_USER}/${backendimage}:latest
                        """

                        // logout 
                        sh "docker logout"
                    }
                }
            }
        }

        stage('Build Docker Image objectifplus -- frontend') {
            steps {
                echo "==> Build de l'image Docker locale"

                /* On construit l'image Docker en tag 'latest'
                   Le Dockerfile doit être à la racine du repo */
                sh """
                    docker build -t ${frontendimage}:latest ${frontendF}
                """
            }
        }

        stage('Push Docker Image emp -- frontend') {
            steps {
                echo "==> Push de l'image sur Docker Hub (tag: latest)"

                /* On utilise les identifiants Jenkins ''
                   ATTENTION :
                   - usernameVariable = ton username Docker Hub
                   - passwordVariable = le mot de passe / token Docker Hub
                */
                script {
                    withCredentials([usernamePassword(
                        credentialsId: 'dockerhub-cred',
                        usernameVariable: 'DOCKERHUB_USER',
                        passwordVariable: 'DOCKERHUB_PASS'
                    )]) {

                        // login Docker Hub
                        sh """
                            echo "${DOCKERHUB_PASS}" | docker login -u "${DOCKERHUB_USER}" --password-stdin
                        """

                        // retag l'image locale avec ton namespace Docker Hub
                        sh """
                            docker tag ${frontendimage}:latest ${DOCKERHUB_USER}/${frontendimage}:latest
                            docker push ${DOCKERHUB_USER}/${frontendimage}:latest
                        """

                        // logout 
                        sh "docker logout"
                    }
                }
            }
        }
        
        stage('execute docker compose') {
            steps {
                echo "==> Exécuter Docker compose"

                /* On construit l'image Docker en tag 'latest'
                   Le Dockerfile doit être à la racine du repo */
                sh """
                    docker compose up -d
                """
            }
        }


    }

    post {
        success {
            echo "Pipeline terminé ya Iness ya janjouna"
        }
    }
}
