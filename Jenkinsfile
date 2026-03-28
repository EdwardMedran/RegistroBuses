pipeline {
    agent any

    environment {
        REPO_DIR     = "C:/DEV_EDWARD/02_PROYECTOS-PERSONALES/html/RegistrosBuses"
        GIT_USER     = "EdwardMedran"
        GIT_EMAIL    = "edwardgino13@gmail.com"
        GITHUB_TOKEN = credentials('github-token')
    }

    stages {

        stage('Checkout') {
            steps {
                echo '📥 Verificando repositorio...'
                bat "git config --global --add safe.directory ${REPO_DIR}"
                bat "cd /d \"${REPO_DIR}\" && git status"
            }
        }

        stage('Validar Archivos') {
            steps {
                echo '🔍 Validando archivos del proyecto...'
                bat """
                    cd /d "${REPO_DIR}"
                    if exist index.html (
                        echo ✅ index.html encontrado
                    ) else (
                        echo ❌ index.html NO encontrado
                        exit 1
                    )
                    if exist README.md (
                        echo ✅ README.md encontrado
                    ) else (
                        echo ⚠️ README.md no encontrado
                    )
                """
            }
        }

        stage('Deploy a GitHub Pages') {
            steps {
                echo '🚀 Desplegando a GitHub Pages...'
                bat """
                    cd /d "${REPO_DIR}"
                    git config user.email "${GIT_EMAIL}"
                    git config user.name "${GIT_USER}"
                    git add .
                    git commit -m "Auto deploy #%BUILD_NUMBER% - %DATE% %TIME%" || echo Nada que confirmar
                    git push https://${GIT_USER}:${GITHUB_TOKEN}@github.com/EdwardMedran/RegistroBuses.git main
                """
            }
        }

        stage('Verificar Deploy') {
            steps {
                echo '✅ Deploy completado exitosamente!'
                echo '🌐 App disponible en: https://edwardmedran.github.io/RegistroBuses'
                bat """
                    cd /d "${REPO_DIR}"
                    git log --oneline -3
                """
            }
        }
    }

    post {
        success {
            echo '========================================='
            echo '✅ PIPELINE EXITOSO'
            echo '🌐 https://edwardmedran.github.io/RegistroBuses'
            echo '========================================='
        }
        failure {
            echo '========================================='
            echo '❌ PIPELINE FALLÓ - Revisar logs arriba'
            echo '========================================='
        }
        always {
            echo "📊 Build #${BUILD_NUMBER} finalizado - ${currentBuild.result}"
        }
    }
}