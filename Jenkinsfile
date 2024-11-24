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
                // Test básico para verificar que la web responde
                sh '''
                    npm install -g serve
                    serve -s build &
                    sleep 5
                    RESPONSE=$(curl -s -o /dev/null -w "%{http_code}" http://localhost:3000)
                    if [ $RESPONSE -eq 200 ]; then
                        echo "Website is responding correctly"
                    else
                        echo "Website is not responding correctly"
                        exit 1
                    fi
                    pkill -f serve
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

