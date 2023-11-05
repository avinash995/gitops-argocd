pipeline{

    agent any
    environment{

        DOCKERHUB_USERNAME = "avinash995"
        APP_NAME = "gitops-argocd-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        REGISTRY_CREDS =  'dockerhub'
    }

    stages{

        stage('cleanup workspace'){
            steps{
                script{
                    cleanWs()
                }
            }
        }
        stage('checkout SCM'){
            steps{
                script{
                    git credentaialsId: 'github',
                    url: 'https://github.com/vikash-kumar01/gitops_argocd_project.git',
                    branch: 'main'
                }
            }
        }
        stage('Build Docker Image'){
            steps{
                script{
                    docker_image = docker.build -f "${IMAGE_NAME}"
                }
            }
        }
        stage('Push Docker Image'){
            steps{
                script{
                    docker.withRegistry('',REGISTRY_CREDS){
                        docker_image.push("$BUILD_NUMBER")
                        docker_image.push('latest')

                    }
                }

            }
        }
    }
}
