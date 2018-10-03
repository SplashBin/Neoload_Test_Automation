pipeline {
    agent any

    stages {
        stage('Deploy NTS') {
            steps {
                echo 'Deploying NTS...'
		sleep '5s'
            }
        }
        stage('Deploy Neoload Web') {
            steps {
                echo 'Deploying Neoload Web...'
		sleep '5s'
            }
        }
        stage('Deploy Neoload Controller') {
            steps {
                echo 'Deploying Neoload Controller...'
		sleep '5s'
            }
        }
	stage('Launching Test') {
            steps {
                echo 'Launching test...'
                sleep '5s'
            }
        }
	stage('Shut Down Neoload Controller') {
            steps {
                echo 'Shuting Down Neoload Controller...'
                sleep '5s'
            }
        }

    }
}
