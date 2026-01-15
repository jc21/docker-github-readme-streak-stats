pipeline {
	triggers {
		cron('0 11 * * 5')
	}
	agent {
		label 'docker'
	}
	options {
		buildDiscarder(logRotator(numToKeepStr: '5'))
		disableConcurrentBuilds()
		ansiColor('xterm')
	}
	environment {
		IMAGE = "github-readme-streak-stats"
	}
	stages {
		stage('Build') {
			steps {
				withCredentials([usernamePassword(credentialsId: 'jc21-dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
					sh 'docker login -u "${DOCKER_USER}" -p "${DOCKER_PASS}"'
					sh './scripts/buildx --push -t "docker.io/jc21/${IMAGE}:latest"'
				}
			}
		}
	}
	post {
		always {
			printResult(true)
		}
	}
}
