pipeline {
    agent any

    stages {
        stage('Deploy Neoload Team Server...') {
          steps {
            sh "curl https://d24mnm5myvorwj.cloudfront.net/documents/download/nts/v2.3/neotys_team_server_2_3_1_linux_x86.sh > nts_installer.sh"
            sh "echo -e 'adminPort$Long=8009\nhttpPort$Long=8084\nlicensePort$Long=7777\nshutdownPort$Long=8005\nsys.adminRights$Boolean=true\nsys.installationDir=./data/NTS2.3.1\nsys.languageId=en\nsys.programGroupDisabled$Boolean=true' > vfile"
            sh "sh nts_installer.sh -q -varfile vfile"
          }
        }
        stage('[ Tmp ] Download script and licences...') {
            steps {
                sh 'git clone https://github.com/SplashBin/Continuous_Testing.git'
                sh 'tar -xvf Apollo_PPRD.tar.gz'   
            }
        }

        stage('Deploy Nl Web') {
          parallel {
            stage('Deploy mongo...') {
              steps {
                sh 'docker run -d --interactive -v /home/bench/data:/data/db --name mongo -p 27017 --hostname mongo mongo:latest'
                echo 'Mongo deployed !'
              }
            }

            stage('Deploy neoload-backend...') {
              steps {
                sh 'sleep 20'
                sh 'docker run -d --interactive -p 8080:1081 \
                      --publish 8081:1082 \
                      --publish 9082:9092 \
                      --env MEMORY_MAX=1500m \
                      --env MONGODB_HOST=mongo \
                      --env MONGODB_PORT=27017 \
                      --env MONGODB_MAX_POOLSIZE=50 \
                      --env NEOLOAD_WEB_PUBLIC_URL=http://192.168.1.31/ \
                      --env FILE_STORAGE_ROUTER_BASE_URL=http://192.168.1.31:8081 \
                      --env FILE_PROJECT_MAX_SIZE_IN_BYTES=100000000 \
                      --env NLPROJECT_MAX_UPLOADED_FILES_PER_WEEK=250 \
                      --link mongo \
                      --name nlweb-backend neotys/neoload-web-backend:latest'
                echo 'Neoload-backend deployed !'
              }
            }

            stage('Deploy neoload-frontend...') {
              steps {
                sh 'sleep 120'
                sh 'docker run -d -e MEMORY_MAX=896m \
                        -e SEND_USAGE_STATISTICS=true \
                        --publish 80:9090 \
                        --publish 81:9091 \
                        --link nlweb-backend \
                        --interactive \
                        --name nlweb-frontend neotys/neoload-web-frontend:latest'
                sh 'sleep 60'
                echo 'Neoload-frontend deployed !'
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