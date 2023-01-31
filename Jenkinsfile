node {   
    stage('Clone repository') {
        git credentialsId: 'git', url: 'https://github.com/padomay1352/marklog.git'
    }
    
    stage('Build image') {
       dockerImage = docker.build("padomay1352/marklog:latest")
    }
    
 stage('Push image') {
        withDockerRegistry([ credentialsId: "dockerhubaccount", url: "" ]) {
        dockerImage.push()
        }
    }    
}