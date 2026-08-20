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
		            powershell '''
		                $DOCKERHUB_TOKEN | docker login -u $DOCKER_USERNAME --password-stdin
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
                    bat "mvn clean install"
                    bat "docker build -t nouran10/myapp-backend . --no-cache"
                    bat "docker push nouran10/myapp-backend"
                }
            }
        }
        stage("Run docker compose") {
            steps {
                dir("formation_level_one") {
                    bat "docker compose down --volumes"
                    bat "docker compose pull"
                    bat "docker compose up -d"
                }
            }
        }
    }
}
