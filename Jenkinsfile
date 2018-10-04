pipeline {
    agent any

    stages {
        stage('Deploy NTS') {
            steps {
                echo 'Deploying NTS'
                sh 'sleep 5'
            }
        }

        stage('Deploy Nl Web') {
            agent {
                docker {
                    image 'mongo:3.4.13'
                    args '--hostname mongo \
                          -v /home/bench/data:/data/db \
                          --tty true \
                          --stdin_open true'
                    label 'mongo'
                }
            }
            agent {
                docker {
                    image 'neotys/neoload-web-backend:latest'
                    args '-p 8080:1081 \
                          -p 8081:1082 \
                          -p 9082:9092 \
                          -e MEMORY_MAX=1500m \
                          -e MONGODB_HOST=mongo \
                          -e MONGODB_PORT=27017 \
                          -e MONGODB_MAX_POOLSIZE=50 \
                          -e NEOLOAD_WEB_PUBLIC_URL=http://neoload.yourcompany.com/ \
                          -e FILE_STORAGE_ROUTER_BASE_URL=http://neoload.yourcompany.com:8081 \
                          -e FILE_PROJECT_MAX_SIZE_IN_BYTES=100000000 \
                          -e NLPROJECT_MAX_UPLOADED_FILES_PER_WEEK=250 \
                          --links mongo \
                          --tty true \
                          --stdin_open true'
                    label 'nlweb-backend'
                }
            }
            agent {
                docker {
                    image 'neotys/neoload-web-frontend:latest'
                    args'-e MEMORY_MAX=896m \
                         -e SEND_USAGE_STATISTICS=true \
                         --links nlweb-backend \
                         --tty true \
                         --stdin_open true'
                    label 'nlweb-frontend'
                }
            }
        }

        stage('Deploy Nl Controller') {
            steps {
                echo 'Testing..'
            }
        }
    }
}

