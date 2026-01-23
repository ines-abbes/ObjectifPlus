pipeline {
    /* On dit à Jenkins d'exécuter le pipeline sur n'importe quel agent.
       ⚠ Cet agent doit avoir Docker installé et le droit d'exécuter "docker build / docker push". */
    agent any
      

    /* Variables globales */
    environment {
        // Nom local de l'image 
        backendimage = "backend"
        frontendimage = "frontend"
        //SONAR_TOKEN = credentials('sonar-token')
        
        // Repo folders
        
        backendF = "backend"
        frontendF = "frontend"

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

        stage('Debug PATH') {
            steps {
                bat """
                    echo PATH=%PATH%
                     where sonar-scanner
                """
    }
}

        stage('SonarQube Backend') {
            steps {
                dir('backend') {
                    
                    // docker run --rm ^
                    //   -e SONAR_HOST_URL=http://host.docker.internal:9000 ^
                    //   -e SONAR_LOGIN=%SONAR_TOKEN% ^
                    //   -v %cd%:/usr/src ^
                    //   sonarsource/sonar-scanner-cli
                bat """ 
                sonar-scanner.bat -D"sonar.projectKey=objplus-backend" -D"sonar.sources=." -D"sonar.host.url=http://localhost:9000" -D"sonar.login=271efa2a1a01bd56d6c4263f70581253b58a65c2"
                """
                }
            }
        }
    //     stage('Build Docker Image objectifplus -- backend') {
    //         steps {
    //             echo "==> Build de l'image Docker locale"

    //             /* On construit l'image Docker en tag 'latest'
    //                Le Dockerfile doit être à la racine du repo */
    //             bat """
    //                 docker build -t ${backendimage}:latest ${backendF}
    //             """
    //         }
    //     }

    //     stage('Push Docker Image objectifplus -- backend') {
    //         steps {
    //             echo "==> Push de l'image sur Docker Hub (tag: latest)"

    //              script {
    //                 withCredentials([usernamePassword(
    //                 credentialsId: 'dockerhub-cred',
    //                 usernameVariable: 'DOCKERHUB_USER',
    //                 passwordVariable: 'DOCKERHUB_PASS'
    //             )]) {
    //                     bat '''
    //                         echo %DOCKERHUB_PASS% | docker login -u %DOCKERHUB_USER% --password-stdin
    //                     '''
                    
    //                     // retag l'image locale avec ton namespace Docker Hub
    //                     bat '''
    //                         docker tag %backendimage%:latest %DOCKERHUB_USER%/%backendimage%:latest
    //                         docker push %DOCKERHUB_USER%/%backendimage%:latest
    //                     '''

                      
    //                     // logout 
    //                     bat "docker logout"
    //                 }
    //             }
    //         }
    //     }

    //     stage('Build Docker Image objectifplus -- frontend') {
    //         steps {
    //             echo "==> Build de l'image Docker locale"

    //             /* On construit l'image Docker en tag 'latest'
    //                Le Dockerfile doit être à la racine du repo */
    //             bat """
    //                 docker build -t ${frontendimage}:latest ${frontendF}
    //             """
    //         }
    //     }

    //     stage('Push Docker Image emp -- frontend') {
    //         steps {
    //             echo "==> Push de l'image sur Docker Hub (tag: latest)"

    //              script {
                                    
    //                 withCredentials([usernamePassword(
    //                     credentialsId: 'dockerhub-cred',
    //                     usernameVariable: 'DOCKERHUB_USER',
    //                     passwordVariable: 'DOCKERHUB_PASS'
    //                 )]) {

    //                     // login Docker Hub
    //                     bat '''
    //                         echo %DOCKERHUB_PASS% | docker login -u %DOCKERHUB_USER% --password-stdin
    //                     '''

    //                     // retag l'image locale avec ton namespace Docker Hub
    //                     bat """
    //                         docker tag %frontendimage%:latest %DOCKERHUB_USER%/%frontendimage%:latest
    //                         docker push %DOCKERHUB_USER%/%frontendimage%:latest
                           
    //                     """

    //                     // logout 
    //                     bat "docker logout"
    //                 }
    //             }
    //         }
    //     }
        
    //     stage('execute docker compose') {
    //         steps {
    //             echo "==> Exécuter Docker compose"

    //             /* On construit l'image Docker en tag 'latest'
    //                Le Dockerfile doit être à la racine du repo */
    //             bat """
    //                 docker compose up -d
    //             """
    //         }
    //     }


    // }

    // post {
    //     success {
    //         echo "Bravo ya Iness ya janjounaaaaa!!!!"
    //     }
    // }
}}
