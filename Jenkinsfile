pipeline {
	agent none

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
			agent { label 'linux' }
			steps {
				sh 'make build'
			}
		}

		stage ('Test') {
			agent { label 'linux' }
			steps {
				sh 'make test'
			}
		}
	}
}
