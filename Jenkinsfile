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
				sh 'docker build -t padomay1352/marklog_backend:latest ./marklog-backend'
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


node{
    def remote = [:]
    remote.name = 'marklog-was'
    remote.host = 'marklog.kro.kr'
    remote.allowAnyHosts = true
    withCredentials([usernamePassword(credentialsId: 'azurewas_ssh', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
        remote.user = userName
        remote.password = identity
        stage('Remote SSH') {
            sshPut remote: remote, from: 'docker-compose.yml', into: '.'
            sshCommand remote: remote, command: 'echo azcom@31337D | docker login -u padomay1352 --password-stdin'
            sshCommand remote: remote, command: 'docker-compose up -d --build'
        }
    }
}