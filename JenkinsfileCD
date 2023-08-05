pipeline{

    agent any

    environment{

        APP_NAME = "gitops-argo-app"

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
                    url: 'https://github.com/siddharth201983/gitops-argocd-cd.git',
                    branch: 'main'
                }
            }
        }
        stage('Update k8s manifest files'){
            steps{
                script{
                    sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                    """
                }
            }
        }
        stage('Push the changed deployment file to Git'){
            steps{
                script{
                    sh """
                    git config --global user.name "siddharth201983"
                    git config --global user.email "sharma.siddharth2009@gmail.com"
                    git add deployment.yaml
                    git commit -m "updated the deployment file"
                    """
                    withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/siddharth201983/gitops-argocd-cd.git main"
                  }
                }
            }
        }
    }
}
