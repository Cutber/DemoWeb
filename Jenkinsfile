pipeline {
    agent any

    environment {
<<<<<<< HEAD
        GH_PAT = credentials('github-pat')  // Usa la credencial almacenada en Jenkins
        REPO_URL = 'https://github.com/Cutber/DemoWeb.git'
        FEATURE_BRANCH = 'feature/dev'
        DEVELOP_BRANCH = 'develop'
    }

    stages {
        stage('Clonar Repositorio') {
            steps {
                script {
                    sh "git clone ${REPO_URL}"
                    sh "cd DemoWeb && git checkout ${FEATURE_BRANCH} && git pull origin ${FEATURE_BRANCH}"
                }
            }
        }

        stage('Obtener Última Versión') {
            steps {
                script {
                    def latestTag = sh(script: "cd DemoWeb && git tag --sort=-v:refname | tail -n 1", returnStdout: true).trim()
                    if (latestTag == '') {
                        latestTag = 'v1.0.0'
                    } else {
                        def parts = latestTag.replace("v", "").split("\\.")
                        latestTag = "v${parts[0]}.${parts[1]}.${parts[2].toInteger() + 1}"
                    }
                    env.NEW_VERSION = latestTag
                    sh "echo 'Nueva versión: ${env.NEW_VERSION}'"
                }
            }
        }

        stage('Crear Pull Request en GitHub') {
            steps {
                script {
                    sh """
                        cd DemoWeb
                        git checkout ${FEATURE_BRANCH}
                        git push origin ${FEATURE_BRANCH}
                        curl -X POST -H "Authorization: token ${GH_PAT}" \
                            -d '{
                                "title": "Merge ${FEATURE_BRANCH} into ${DEVELOP_BRANCH}",
                                "head": "${FEATURE_BRANCH}",
                                "base": "${DEVELOP_BRANCH}",
                                "body": "Pull request automático creado por Jenkins"
                            }' \
                            https://api.github.com/repos/Cutber/DemoWeb/pulls
                    """
=======
        REPO_URL = 'https://github.com/Cutber/DemoWeb.git'
        BRANCH = 'feature/dev'
        REPORT_DIR = 'C:\\Users\\eTriber\\OneDrive\\Documentos\\generador_reportes\\Reportes_Relacionados'
    }

    stages {
        stage('Clonar Repositorio o Actualizar Código') {
            steps {
                script {
                    bat '''
                    if exist "DemoWeb" (
                        echo "📌 El repositorio ya existe. Haciendo git pull..."
                        cd DemoWeb
                        git reset --hard
                        git pull origin %BRANCH%
                    ) else (
                        echo "📌 Clonando el repositorio..."
                        git clone -b %BRANCH% %REPO_URL%
                    )
                    '''
                }
            }
        }

        stage('Configurar Entorno de Python') {
            steps {
                script {
                    bat '''
                    cd DemoWeb
                    if not exist "venv" (
                        echo "📌 Creando entorno virtual..."
                        python -m venv venv
                    )
                    echo "📌 Activando entorno virtual..."
                    call venv\\Scripts\\activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                    '''
                }
            }
        }

        stage('Ejecutar Script de Reporte') {
            steps {
                script {
                    bat '''
                    cd DemoWeb\\reportes
                    call ..\\venv\\Scripts\\activate
                    python reporte_relacionados.py
                    '''
                }
            }
        }

        stage('Verificar Reporte Generado') {
            steps {
                script {
                    bat "dir /B %REPORT_DIR%"  // Lista los archivos generados en la carpeta
                }
            }
        }

        stage('Opcional: Commit y Push de Cambios') {
            when {
                expression { return params.HACER_COMMIT }
            }
            steps {
                script {
                    bat '''
                    cd DemoWeb
                    git add .
                    git commit -m "🚀 Actualización automática desde Jenkins"
                    git push origin %BRANCH%
                    '''
>>>>>>> 6ac3940 (🚀 Actualización de Jenkinsfile para Windows 11)
                }
            }
        }
    }
<<<<<<< HEAD
}
=======

    post {
        success {
            echo '✅ Pipeline ejecutado exitosamente en Windows 11.'
        }
        failure {
            echo '❌ Hubo un error en la ejecución del pipeline.'
        }
    }
}
>>>>>>> 6ac3940 (🚀 Actualización de Jenkinsfile para Windows 11)
