pipeline {
    agent any

    stages {
        stage('Deploy NTS') {
            steps {
                echo 'Deploying NTS...'
		sh 'sleep 5'
            }
        }
        stage('Deploy Neoload Web') {
            steps {
                echo 'Deploying Neoload Web...'
		sh 'sleep 5'
            }
        }
        stage('Deploy Neoload Controller') {
            steps {
                echo 'Deploying Neoload Controller...'
		sh 'sleep 5'
            }
        }
	stage('Launching Test') {
            steps {
                echo 'Launching test...'
                sh 'sleep 5'
            }
        }
	stage('Shut Down Neoload Controller') {
            steps {
                echo 'Shuting Down Neoload Controller...'
                sh 'sleep 5'
            }
        }

    }
}
