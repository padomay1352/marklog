pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('dk')
	}

	stages {
        stage('Git submodule update'){
            steps{
                sh 'git submodule init'
                sh 'git submodule update --remote'
            }
        }
            

		stage('Build frontend & backend') {
			steps {
				sh 'docker build -t padomay1352/marklog_frontend:latest ./marklog-frontend'
				sh 'docker build -t padomay1352/marklog_backend:latest ./marklog_backend'
			}
		}

		stage('Login') {
			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push') {
			steps {
				sh 'docker push padomay1352/marklog_frontend:latest'
				sh 'docker push padomay1352/marklog_backend:latest'
			}
		}

	}
	post {
		always {
			sh 'docker logout'
		}
	}
}