node {
	def application = "springbootapp"
	def dockerhubaccountid = "sathishsubramanian"
	stage('Clone repository') {
		checkout scm
	}

	stage('Build image') {
		app = docker.build("${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
	}

	stage('Push image') {
		withDockerRegistry([ credentialsId: "dockerHub", url: "" ]) {
		app.push()
		app.push("latest")
	}
	}

	stage('Deploy') {
		bat "docker run -d -p 81:8080 -v /var/log/:/var/log/ ${dockerhubaccountid}/${application}:${BUILD_NUMBER}" 
	}
	
	stage('Remove old images') {
		// remove docker pld images
		bat "docker rmi ${dockerhubaccountid}/${application}:latest -f" 
   }
}
