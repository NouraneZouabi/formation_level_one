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
                bat '''
                    echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                    docker build -t %DOCKER_USER%/angular-app:latest .
                    docker push %DOCKER_USER%/angular-app:latest
                '''
            }
        }
    }
}
stage('Generate backend image') {
    steps {
        dir('formation_level_one/spring-app') {
            withCredentials([usernamePassword(
                credentialsId: 'docker-hub-creds',
                usernameVariable: 'DOCKER_USER',
                passwordVariable: 'DOCKER_PASS'
            )]) {
                bat '''
                    echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                    docker build -t %DOCKER_USER%/spring-app:latest .
                    docker push %DOCKER_USER%/spring-app:latest
                '''
            }
        }
    }
}


    }
}
