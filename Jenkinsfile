pipeline {
    agent any

    stages {

        stage('Clean up') {
            steps {
                deleteDir()
            }
        }

        stage('Clone repo') {
            steps {
                bat 'git clone https://github.com/NouraneZouabi/formation_level_one.git'
            }
        }

        stage('Generate frontend image') {
            steps {
                dir('formation_level_one/angular-app') {

                    withCredentials([usernamePassword(
                        credentialsId: 'docker-hub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )]) {

                        bat 'echo %DOCKERHUB_PASSWORD% | docker login -u %DOCKERHUB_USERNAME% --password-stdin'

                        bat 'docker build -t nouran10/myapp-frontend . --no-cache'

                        bat 'docker push nouran10/myapp-frontend'
                    }
                }
            }
        }

        stage('test sonar') {
            steps {
                dir('formation_level_one/springboot/app') {
                    bat 'set "MAVEN_USER_HOME=C:\\Jenkins\\.m2" && mvnw.cmd clean install'
                    bat """
                        mvnw.cmd clean verify sonar:sonar \
                          -Dsonar.projectKey=deploy-appa \
                          -Dsonar.host.url=http://54.196.35.185:9000 \
                          -Dsonar.login=sqp_2c3f83231f1adf1fb169bbd17260bb20b8438a9a 
                    """
                }
            }
        }
      
        stage('Generate backend image') {
            steps {
                dir('formation_level_one/springboot/app') {

                    withCredentials([usernamePassword(
                        credentialsId: 'dockeer-cred',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )]) {

                        bat 'echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin'

                        bat 'set "MAVEN_USER_HOME=C:\\Jenkins\\.m2" && mvnw.cmd clean install'

                        bat 'docker build -t nouran10/myapp-backend . --no-cache'

                        bat 'docker push nouran10/myapp-backend'
                    }
                }
            }
        }


    }
}
