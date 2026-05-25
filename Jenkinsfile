pipeline {
    agent any

    options {
        // 1. Opciones del Job
        disableConcurrentBuilds()
        timestamps()
        timeout(time: 5, unit: 'MINUTES')
    }

    environment {
        // 2. Variables de entorno
        FORCE_COLOR = '0'
        NO_COLOR    = 'true'
    }

    stages {
        // 3. Auditoría de herramientas
        stage('Audit tools') {
            steps {
                sh 'node --version'
            }
        }

        // 4. Instalación de dependencias
        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }

        // 5. Creación de ficheros autogenerados
        stage('Generate files') {
            steps {
                sh 'npm run prisma:generate'
            }
        }

        // 6. Chequeo de formato de código
        stage('Format check') {
            steps {
                sh 'npm run format:check'
            }
        }

        // 7. Chequeo de calidad de código
        stage('Code quality') {
            steps {
                sh 'npm run lint'
            }
        }

        // 8. Chequeo de tipos
        stage('Type check') {
            steps {
                sh 'npm run type-check'
            }
        }

        // 9. Ejecución de tests
        stage('Tests') {
            steps {
                sh 'npm run test'
            }
        }

        // 10. Construcción y archivado
        stage('Build') {
            steps {
                sh 'npm run build'
                // Archiva el directorio dist/ activando el fingerprinting
                archiveArtifacts artifacts: 'dist/**', fingerprint: true
            }
        }
    }

    // 11. Etapas finales (Post-actions)
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Review logs.'
        }
        always {
            cleanWs()
        }
    }
}
