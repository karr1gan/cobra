// Uses Declarative syntax to run commands inside a container.
pipeline {
    agent {
        kubernetes {
            // Rather than inline YAML, in a multibranch Pipeline you could use: yamlFile 'jenkins-pod.yaml'
            // Or, to avoid YAML:
            // containerTemplate {
            //     name 'shell'
            //     image 'go1.17.2'
            //     command 'sleep'
            //     args 'infinity'
            // }
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: golang
    image: golang:1.17
    command:
    - sleep
    args:
    - infinity
'''
            // Can also wrap individual steps:
            // container('shell') {
            //     sh 'hostname'
            // }
            defaultContainer 'golang'
        }
    }
    stages {
        stage('Download dependencies') {
            steps {
                sh 'echo "display go.mod: " && cat go.mod'
		sh 'echo "display go.sum: " && cat go.sum'
		sh 'go get ./...'
            }
	}
	stage('Build') {
            steps {
                sh 'mkdir ./dist'
		sh '''CGO_ENABLED=0 go build -o ./dist/cobracli -a -ldflags '-w -extldflags "-static"' ./cobra/main.go'''  
			// -o zapisz wynik do katalogu ./dist/cobracli
			// CGO_ENABLED=0 wylacza zaleznosci z jezyka C w GO
			// -a ==
			// -ldflags == flagi dla linkera
			// -w -extldflags "-static"  ==
                sh 'ls -la ./dist'
	    }
        }
    }
}


