# Readme


This would fail because we didnt specify an agent for the build to run on
```groovy
pipeline {
	agent none

	stages {
		stage ('Build') {
			steps {
				sh 'make build'
			}
		}
	}
}
```
