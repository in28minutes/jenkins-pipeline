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

pipeline {
	agent { docker {image 'maven:3.3.3'}}
	stages {
		stage('build') {
			steps {
				sh 'mvn --version'
			}
		}
	}
}