pipeline {
    agent {
        label 'jenkins-agent'
    }
    
    environment {
        WEBSERVER_HOST = 'webserver'
        WEBSERVER_PATH = '/usr/share/nginx/html'
    }

    stages {
        stage('Source') {
            steps {
                // Limpiamos el workspace
                cleanWs()
                // Clonamos el repositorio
                git branch: 'main', 
                    url: 'https://github.com/Cloud-DevOps-Labs/jenkins-playground-app.git'
            }
        }
        
        stage('Build') {
            steps {
                // Instalamos dependencias y construimos
                sh '''
                    npm install
                    npm run build
                '''
            }
        }
        
        stage('Test') {
            steps {
                sh '''
                    if [ ! -f "build/index.html" ]; then
                        echo "Error: index.html no encontrado"
                        exit 1
                    fi
                    
                    if ! grep -q "<html" "build/index.html"; then
                        echo "Error: El archivo no parece ser HTML válido"
                        exit 1
                    fi
                    
                    echo "Tests completados exitosamente"
                '''
            }
        }
        
        stage('Package') {
            steps {
                // Empaquetamos los archivos
                sh '''
                    rm -rf www
                    mkdir www
                    cp -r build/* www/
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                // Desplegamos al servidor web via SSH
                sshagent(['webserver-key']) {
                    sh '''
                        ssh-add -l
                        env | grep SSH
                        ssh -v -o StrictHostKeyChecking=no root@${WEBSERVER_HOST}
                        ssh -o StrictHostKeyChecking=no root@${WEBSERVER_HOST} "rm -rf ${WEBSERVER_PATH}/*"
                        scp -r www/* root@${WEBSERVER_HOST}:${WEBSERVER_PATH}/
                    '''
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
