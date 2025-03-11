pipeline {
    agent any

    environment {
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
                }
            }
        }
    }
}
