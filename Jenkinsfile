pipeline {
    agent any

    stages {

        stage("Clean up") {
            steps {
                deleteDir()
            }
        }

        stage("Clone repo") {
            steps {
                bat "git clone https://github.com/NouraneZouabi/formation_level_one.git"
            }
        }

		stage("Docker Login") {
		    steps {
		        withCredentials([
		            usernamePassword(
		                credentialsId: 'docker-hub-creds',
		                usernameVariable: 'DOCKER_USERNAME',
		                passwordVariable: 'DOCKERHUB_TOKEN'
		            )
		        ]) {
		            bat '''
		                @echo off
		                docker login -u "%DOCKER_USERNAME%" --password "%DOCKERHUB_TOKEN%"
		            '''
		        }
		    }
		}

        stage("Generate frontend image") {
            steps {
                dir("formation_level_one/angular-app") {
                    bat "docker build -t nouran10/myapp-frontend . --no-cache"
                    bat "docker push nouran10/myapp-frontend"
                }
            }
        }

        stage("Generate backend image") {
            steps {
                dir("formation_level_one/springboot/app") {
                    bat 'set "MAVEN_USER_HOME=C:\\Jenkins\\.m2" && mvnw.cmd clean install'
                    bat "docker build -t nouran10/myapp-backend . --no-cache"
                    bat "docker push nouran10/myapp-backend"
                }
            }
        }

        stage("Deploy avec k8s") {
            steps {
                dir("formation_level_one") {
					withKubeConfig([ credentialsId: "kubeconfigcred", serverUrl: "http://35.170.6.184:6443"]) {
						bat " kubectl get nodes "
	                    bat "kubectl apply -f k8s"
						bat "kubectl apply -f ingress.yaml"
					}
                }
            }
        }

    }
}
