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


// CHECK THIS OUT - post {

// sh 'echo "Fail!"; exit 1'

//This Snippet Generator will help you learn the Pipeline Script code which can be used to define various steps. Pick a step you are interested in from the list, configure it, click Generate Pipeline Script, and you will see a Pipeline Script statement that would call the step with that configuration. You may copy and paste the whole statement into your script, or pick up just the options you care about. (Most parameters are optional and can be omitted in your script, leaving them at default values.)
//checkout, echo, git, input, retry, sleep, timeout

//Global variables

// echo "$PATH"
// 				echo "$env.BUILD_NUMBER"
// 				echo "$env.BUILD_ID"
// 				echo "$env.JOB_NAME"
// 				echo "$env.BUILD_TAG"
// 				echo "$env.BUILD_URL"

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
				echo "$PATH"
				echo "$env.BUILD_NUMBER"
				echo "$env.BUILD_ID"
				echo "$env.JOB_NAME"
				echo "$env.BUILD_TAG"
				echo "$env.BUILD_URL"
			}
		}

		stage('Build'){
			steps {
				sh "mvn clean compile"
			}
		}

		// stage('Test'){
		// 	steps {
		// 		sh "mvn test"
		// 	}
		// }

		// stage('Integration Test'){
		// 	steps {
		// 		sh "mvn failsafe:integration-test failsafe:verify"
		// 	}
		// }

		stage('Package'){
			steps {
				sh "mvn package -DskipTests"
				archiveArtifacts artifacts: 'target/*', fingerprint: true
			}	
		}

		stage('Sonar') {
			steps {
				sh "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.6.0.1398:sonar -Dsonar.host.url=http://sonarqube:9000"
			}
		}

		stage('Build Docker Image') {
			steps {
				//sh "docker build -t rangakaranam/currency-exchange-azure:$env.BUILD_TAG  --pull --no-cache ."
				script {
					dockerImage = docker.build("rangakaranam/currency-exchange-azure:${env.BUILD_ID}")
				}
			}
			
		}

		stage('Push Docker Image') {
			steps {
				script {
					docker.withRegistry( '', 'dockerhub' ) {
						dockerImage.push()
						dockerImage.push('latest')
					}
				}
			}
			
		}

	}
	post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }

}