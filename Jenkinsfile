pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "mi-aplicacion:latest"
        K8S_NAMESPACE = "produccion"
        DEPLOYMENT_NAME = "mi-app-deployment"
        REGISTRY_URL = "mi-registro-docker.com"
        REGISTRY_CREDENTIALS = "docker-credentials"
    }

    stages {
        stage('Checkout Código') {
            steps {
                git branch: 'main', url: 'https://github.com/mi-org/mi-repo.git'
            }
        }

        stage('Construcción y Push de Imagen Docker') {
            steps {
                script {
                    docker.withRegistry("https://${REGISTRY_URL}", REGISTRY_CREDENTIALS) {
                        sh "docker build -t ${REGISTRY_URL}/${DOCKER_IMAGE} ."
                        sh "docker push ${REGISTRY_URL}/${DOCKER_IMAGE}"
                    }
                }
            }
        }

        stage('Despliegue en Kubernetes') {
            steps {
                script {
                    withKubeConfig([credentialsId: 'k8s-credentials']) {
                        sh """
                        kubectl set image deployment/${DEPLOYMENT_NAME} \
                        ${DEPLOYMENT_NAME}=${REGISTRY_URL}/${DOCKER_IMAGE} -n ${K8S_NAMESPACE}
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo '¡Despliegue exitoso!'
        }
        failure {
            echo 'Hubo un error en el despliegue.'
        }
    }
}
