pipeline {
    agent any

    environment {
        DEPLOY_DIR = '/var/www/html'
        BRANCH = 'main'
        REPO = 'https://github.com/edwinsgeorge/jenkins.git'
    }

    stages {

        stage('Clone / Update Code') {
            steps {
                script {
                    if [ -d "${DEPLOY_DIR}/.git" ]; then
                        sh """
                        cd ${DEPLOY_DIR}
                        echo "Existing repo found"
                        git fetch origin ${BRANCH}
                        git reset --hard origin/${BRANCH}
                        """
                    } else {
                        sh """
                        echo "Cloning fresh repo"
                        git clone -b ${BRANCH} ${REPO} ${DEPLOY_DIR}
                        """
                    }
                }
            }
        }

        stage('Restart Web Server') {
            steps {
                sh """
                sudo systemctl restart httpd
                systemctl is-active --quiet httpd && echo "httpd is running"
                """
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful"
        }
        failure {
            echo "❌ Deployment failed"
        }
    }
}
