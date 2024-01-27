pipeline {

	environment {
	dockerImage = "rohithmarigowda/assignment"
        dockerTag = "${BUILD_NUMBER}"
    }
    
    agent any
    stages {
        stage('Git Checkout') {
            steps {
		    //git 'https://github.com/rohith-marigowda/cloudWithRaja.git'
		    git branch: 'main', changelog: false, poll: false, url: 'https://github.com/rohith-marigowda/cloudWithRaja'
            }
        }

	stage('Run Unit and Integration tests'){
  	    steps {
		echo 'In this step run unit and integration tests'
	    }
	 }

	stage('SonarQube analysis') {
        steps{
        //withSonarQubeEnv('sonarqube-9.7.1') { 
        sh 'echo execute below command for sonar code analysis'
		//sh "mvn sonar:sonar"
    //}
        }
        }    

        stage('Docker Build') {
            steps {
		  sh 'docker image build -t $dockerImage:$dockerTag .'
            }
        }

        stage('Docker Push') {
            steps {
                	withCredentials([usernamePassword(
             credentialsId: "dockerhub",
             usernameVariable: "dockerUser",
             passwordVariable: "dockerPassword"
     )]) {
         sh "docker login -u '$dockerUser' -p '$dockerPassword'"
     }
     	sh "docker image push $dockerImage:$dockerTag"
            }
        }

	stage('Trigger ManifestUpdate') {
                echo "triggering updatemanifestjob"
                build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
    }
}
