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
stage('Test Docker Credentials') {
    steps {
        withCredentials([usernamePassword(
            credentialsId: 'docker-hub-creds',
            usernameVariable: 'DOCKER_USER',
            passwordVariable: 'DOCKER_PASS'
        )]) {
            powershell '''
                Write-Host "User Jenkins:"
                whoami

                Write-Host "Docker user length:"
                Write-Host $env:DOCKER_USER.Length

                Write-Host "Docker password length:"
                Write-Host $env:DOCKER_PASS.Length

                Write-Host "Testing Docker login..."

                $env:DOCKER_PASS | docker login --username $env:DOCKER_USER --password-stdin
            '''
        }
    }
}
        stage('Generate frontend image') {
            steps {
                dir('formation_level_one/angular-app') {
                    withCredentials([usernamePassword(
                        credentialsId: 'dockeer-crd',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )]) {
                        bat '''
                            echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                            if errorlevel 1 exit /b 1
                        
                            docker build -t %DOCKER_USER%/angular-app:latest .
                            if errorlevel 1 exit /b 1
                        
                            docker push %DOCKER_USER%/angular-app:latest
                            if errorlevel 1 exit /b 1
                        '''
                    }
                }
            }
        }
        stage('Generate backend image') {
            steps {
                dir('formation_level_one/spring-app') {
                    withCredentials([usernamePassword(
                        credentialsId: 'dockeer-crd',
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

        stage("Lancement du docker compose "){
          steps {
            dir("formation_level_one/"){
              bat "docker compose down --volumes "
              bat "docker compose pull"
              bat "docker compose up -d "
            }
          }
        }
    }
}
