pipeline {

	environment {
	dockerImage = "rohithmarigowda/assignment"
        dockerTag = "${BUILD_NUMBER}"
    }
    
    agent any
    stages {
        stage('Git Checkout') {
            steps {
		    git 'https://github.com/rohith-marigowda/cloudWithRaja.git'
            }
        }

	stage('Run Unit and Integration tests'){
  	    steps {
		echo 'In this step run unit and integration tests'
	    }
	 }

	stage('SonarQube analysis') {
        steps{
        withSonarQubeEnv('sonarqube-9.7.1') { 
        sh 'echo execute below command for sonar code analysis'
		//sh "mvn sonar:sonar"
    }
        }
        }    

        stage('Docker Build') {
            steps {
		    script{
			    docker.build('$dockerImage:$dockerTag')
		    }
            }
        }

        stage('Docker Push') {
            steps {
		    script{
			docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                	docker.image('$dockerImage:$dockerTag').push()
                    }
		    }
            }
        }
    }
}
