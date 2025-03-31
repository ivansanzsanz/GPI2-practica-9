pipeline {
    agent any  // Esto indica que el pipeline puede ejecutarse en cualquier agente disponible

    environment {
        // Aquí puedes definir variables de entorno, si las necesitas
        BRANCH_NAME = 'feature'  // Definir la rama a usar
    }

    stages {
        // Etapa de Checkout: obtener el código de la rama "feature"
        stage('Checkout') {
            steps {
                script {
                    // Realiza un checkout de la rama "feature"
                    git branch: env.BRANCH_NAME, url: 'https://github.com/usuario/repo.git'
                }
            }
        }

        // Etapa de Build: compilar el proyecto
        stage('Build') {
            steps {
                script {
                    // Ejecuta la construcción del proyecto (ajustar el comando según el tipo de proyecto)
                    sh 'npm install'  // Para proyectos Node.js, usa el comando adecuado según tu entorno
                }
            }
        }

        // Etapa de Testing: ejecutar las pruebas unitarias
        stage('Test') {
            steps {
                script {
                    // Ejecuta las pruebas del proyecto (ajustar el comando según el entorno de pruebas)
                    sh 'npm test'  // Comando para ejecutar pruebas en un proyecto Node.js
                }
            }
        }
    }

    post {
        always {
            // Este bloque se ejecuta después de la ejecución del pipeline, sin importar si tuvo éxito o no
            echo 'Pipeline completado.'
        }
        success {
            // Este bloque se ejecuta solo si el pipeline fue exitoso
            echo 'El pipeline fue exitoso.'
        }
        failure {
            // Este bloque se ejecuta si el pipeline falla
            echo 'El pipeline falló.'
        }
    }
}
