pipeline {
	agent none

	triggers {
		cron('H */4 * * 1-5')
	}

	parameters {
		choice(name: 'ENVIRONMENT', choices: ['dev', 'qa', 'int'], description: 'Which environment to test')
	}

	environment {
		ACCESS_KEY = credentials('access-key')
	}

	stages {
		stage ('Install Dependencies') {
			agent any
			steps {
				sh "docker login -u tom.lorentsen -u $ACCESS_KEY"
				sh "make setup"
			}
		}

		stage ('Build') {
			agent {
				label 'linux'
			}
			tools {
				maven 'maven'
			}
			steps {
				sh 'make build'
			}
		}

		stage ('Test') {
			agent {
				label 'linux'
			}
			steps {
				script {
					def browsers = ['chrome', 'firefox']
					for (int i = 0; i < browsers.size(); ++i) {
						echo "Testing the ${browsers[i]} browser"
						sh "BROWSER=${browsers[i]}"
						sh "make test"
					}
				}
			}
		}
		stage ('Deploy') {
			when {
				expression { BRANCH_NAME ==~ /(production|staging)/ }
			}
			steps {
				sh 'env=$ENVIRONMENT make deploy'
			}
		}
	}
}
