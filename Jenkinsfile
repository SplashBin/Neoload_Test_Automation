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
            parallel {
                stage('Deploying mongo...') {
                    agent {
                      docker {
                          image 'mongo:latest'
                          args '-t -i -v /home/bench/data:/data/db --name mongo -h mongo'
                      }
                    }
                    steps {
                      echo 'mongo deployed !'
                    }
                }

                stage('Deploying nl backend...') {
                    agent {
                        steps {
                          sh 'sleep 5'
                        }
                        docker {
                            image 'neotys/neoload-web-backend:latest'
                            args '-t -i -p 8080:1081 \
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
                                  --link mongo \
                                  --name nlweb-backend'
                        }
                    }
                    steps {
                      echo 'nl backend deployed !'
                    }
                }

                stage('Deploying nl frontend...') {
                    agent {
                        docker {
                            image 'neotys/neoload-web-frontend:latest'
                            args'-e MEMORY_MAX=896m \
                                 -e SEND_USAGE_STATISTICS=true \
                                 --link nlweb-backend \
                                 -t true \
                                 -i true \
                                 --name nlweb-frontend'
                        }
                    }
                    steps {
                      echo 'nl frontend deployed !'
                    }
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

