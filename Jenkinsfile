pipeline {
    agent any

    stages {
        stage('Deploy NTS') {
            steps {
                echo 'Deploying NTS'
                sh 'sleep 5'
            }
        }

        stage('Deploy mongo...') {
          agent {
            docker {
              image 'mongo:latest'
              args '--interactive -v /home/bench/data:/data/db --name mongo --hostname mongo --tty'
            }
          }
          steps {
            echo 'Mongo deployed !'
          }
        }

        stage('Deploy Nlweb Backend...') {
          agent {
            docker {
              image 'neotys/neoload-web-backend:latest'
              args '--interactive -p 8080:1081 \
                    --publish 8081:1082 \
                    --publish 9082:9092 \
                    --env MEMORY_MAX=1500m \
                    --env MONGODB_HOST=mongo \
                    --env MONGODB_PORT=27017 \
                    --env MONGODB_MAX_POOLSIZE=50 \
                    --env NEOLOAD_WEB_PUBLIC_URL=http://neoload.yourcompany.com/ \
                    --env FILE_STORAGE_ROUTER_BASE_URL=http://neoload.yourcompany.com:8081 \
                    --env FILE_PROJECT_MAX_SIZE_IN_BYTES=100000000 \
                    --env NLPROJECT_MAX_UPLOADED_FILES_PER_WEEK=250 \
                    --link mongo \
                    --name nlweb-backend -t'
            }
          }
          steps {
            echo 'Nlweb Backend deployed !'
          }
        }

        stage('Deploy Nlweb frontend...') {
          agent {
            docker {
              image 'neotys/neoload-web-frontend:latest'
              args '-e MEMORY_MAX=896m \
                    -e SEND_USAGE_STATISTICS=true \
                    --link nlweb-backend \
                    -t true \
                    -i true \
                    --name nlweb-frontend'
            }
          }
          steps {
            echo 'Nlweb frontend deployed !'
          }
        }

        stage('Deploy Nl Controller') {
            steps {
                echo 'Testing..'
            }
        }
    }
}