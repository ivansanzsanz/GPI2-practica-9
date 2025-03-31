pipeline {
    agent any

    environment {
        BRANCH_NAME = 'feature/desarrollo-practica-9'  // Usa el nombre exacto de tu rama
        REPO_URL = 'https://github.com/ivansanzsanz/GPI2-practica-9.git'  // Usa tu URL real
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: env.BRANCH_NAME]],
                    extensions: [],
                    userRemoteConfigs: [[
                        url: env.REPO_URL,
                        credentialsId: 'tus-credenciales'  // Reemplaza con el ID de tus credenciales en Jenkins
                    ]]
                ])
            }
        }

        stage('Build') {
			steps {
				script {
					// Primero intenta instalar sin --force
					sh 'npm install --force'
					
					// Si falla, intenta con --legacy-peer-deps en lugar de --force
					sh 'npm install --legacy-peer-deps || true'
					
					// Si aún hay problemas con node-sass, instala sass
					sh 'npm uninstall node-sass && npm install sass || true'
				}
			}
		}

        stage('Test') {
            steps {
                script {
                    sh 'npm test'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completado.'
        }
        success {
            echo 'El pipeline fue exitoso.'
        }
        failure {
            echo 'El pipeline falló.'
        }
    }
}
