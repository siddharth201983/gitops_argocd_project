pipeline{

    agent any

    environment{

        DOCKERHUB_USERNAME = "siddharth20"
        APP_NAME = "gitops-argo-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        REGISTRY_CREDS = "dockerhub"


    }

    stages{

        stage('Cleanup workspace'){

            steps{
                script{

                    cleanWs()
                }
            }
        }
        stage('Checkout SCM'){

            steps{
                script{
                    git credentialsId: 'github',
                    url: 'https://github.com/siddharth201983/gitops_argocd_project.git',
                    branch: 'main'
                }
            }
        }
        stage('Build DOcker Image'){
            steps{
                script{

                    docker_image = docker.build "${IMAGE_NAME}"
                }
            }
        }
    }
}