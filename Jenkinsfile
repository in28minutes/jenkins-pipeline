//DELETE TARGET FOLDER
//Admin Password
//Initialize Plugins
//Setup Maven and Docker

//Limited form of Groovy Syntax.

//SCRIPTED PIPELINES
//Node - a machine to run the pipelines
//Stage blocks are optional in scripted pipelines
// node {
// 	stage('Build') {
// 		echo "Build"
// 	}
// 	stage('Test') {
// 		echo "Test"
// 	}
// }

//We are using declarative syntax - All your pipeline definition is in the pipeline block.

//Poll every minute
//* * * * *
//stage - "Build", "Test", "Deploy" - Can be used to visualize progress
// pipeline {
// 	agent any
// 	stages {
// 		stage('Build') {
// 			steps {
// 				echo "Build"
// 			}
// 		}
// 		stage('Test') {
// 			steps {
// 				echo "Test"
// 			}
// 		}
// 	}
// }

//node:6.3
//npm --version
// pipeline {
// 	agent { docker {image 'maven:3.6.3'}}
// 	stages {
// 		stage('build') {
// 			steps {
// 				sh 'mvn --version'
// 			}
// 		}
// 	}
// }

// pipeline {
// 	agent { docker {image 'maven:3.6.3'}}
// 	stages {
// 		stage('build') {
// 			steps {
// 				sh 'mvn --version'
// 			}
// 		}
// 	}
// }


pipeline {

	agent any

	environment {
		dockerHome = tool 'myDocker'
		mavenHome  = tool 'myMaven'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}

	stages {
		
		stage('Checkout') {
			steps {
				checkout scm
			}
		}

		stage('Build'){
			steps {
				sh "mvn clean compile"
			}
		}

		stage('Test'){
			steps {
				sh "mvn test"
			}
		}

		stage('Integration Test'){
			steps {
				sh "mvn failsafe:integration-test failsafe:verify"
			}
		}
	}
}