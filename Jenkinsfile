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
    
    stage('User Acceptance test - AuthService') {
    
    	def response= input message: 'Is this build good to go?',
    		parameters: [choice(choices: 'Yes\nNo', 
    		description: '', name: 'Pass')]
    		
    	if(response=="Yes") {
    		stage('Release - AuthService') {
    			sh 'gradle build -x test'
    			sh 'echo AuthService is ready to release'
    		}
    	}
	}
}

