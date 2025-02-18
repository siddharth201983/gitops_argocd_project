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
        stage('Build Docker Image'){
            steps{
                script{

                    docker_image = docker.build "${IMAGE_NAME}"
                }
            }
        }
        stage('Push Docker Image'){
            steps{
                script{
                    docker.withRegistry('', REGISTRY_CREDS){
                        docker_image.push("$BUILD_NUMBER")
                        docker_image.push('latest')
                    }
                }
            }
        }
        stage('Delete Docker Images'){
            steps{
                script{
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
                }
            }
        }
        stage('Trigger CD Pipelibe'){
            steps{
                script{
                    sh "curl -v -k --user siddharth:1121379df1b472667f5f16774e391b43c4 -X POST -H 'cache-control:no-cache' -H 'content-type:application/x-www-form-urlencoded' --data 'IMAGE_TAG=${IMAGE_TAG}' 'http://34.203.200.32:8080/job/gitops-argocd_CD/buildWithParameters?token=gitops-config'"
                }
            }
        }
    }
}
