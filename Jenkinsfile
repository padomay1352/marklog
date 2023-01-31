pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('dk')
	}

	stages {

		stage('Build') {

			steps {
				sh 'docker build -t padomay1352/marklog:latest .'
			}
		}

		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}
		stage('Push') {
			steps {
				sh 'docker push padomay1352/marklog:latest'
			}
		}
	}
	post {
		always {
			sh 'docker logout'
		}
	}
}