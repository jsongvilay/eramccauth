node {
    stage('Checkout') {
        git branch: 'main', url: 'https://github.com/jsongvilay/eramccauth.git'
    }

    stage('Gradle build - AuthService') {
        sh 'gradle clean build'
    }
    
    stage('Gradle bootJar-Package - AuthService') {
        sh 'gradle bootJar'
    }
    
    stage ('Containerize the app-docker build - AuthService') {
        sh 'docker build --rm -t project-auth:v1.0 .'
    }
    
    stage ('Inspect the docker image - AuthService'){
        sh "docker images project-auth:v1.0"
        sh "docker inspect project-auth:v1.0"
    }
    
    stage ('Run Docker container instance - AuthService'){
        sh "docker run -d --rm --name project-auth -p 8081:8081 project-auth:v1.0"
     }
    
    stage('User Acceptance Test - AuthService') {
	
	  def response= input message: 'Is this build good to go?',
	   parameters: [choice(choices: 'Yes\nNo', 
	   description: '', name: 'Pass')]
	
	  if(response=="Yes") {
	    stage('Deploy to Kubenetes cluster - AuthService') {
	      sh "kubectl create deployment project-auth --image=project-auth:v1.0"
		 //get the value of API_HOST from kubernetes services and set the env variable
	      sh "set env deployment/project-auth API_HOST=\$(kubectl get service/event-data -o jsonpath='{.spec.clusterIP}'):8080"
	      sh "kubectl expose deployment project-auth --type=LoadBalancer --port=8081"
	    }
	  }
    }
    
    stage('Production Deployment View') {
        sh "kubectl get deployments"
        sh "kubectl get pods"
        sh "kubectl get services"
    }
}

