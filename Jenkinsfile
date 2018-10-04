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
          steps {
            sh 'docker run --interactive -v /home/bench/data:/data/db --name mongo --hostname mongo mongo:latest'
          }
        }


        stage('Deploy Nl Controller') {
            steps {
                echo 'Testing..'
            }
        }
    }
}