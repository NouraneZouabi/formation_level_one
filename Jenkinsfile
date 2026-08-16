pipeline {
  agent any

    
  stages {
    stage("clean up"){
      steps {
        deleteDir()
      }
    }
    
    stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'githubcrd',
                    url: 'https://github.com/NouraneZouabi/formation_level_one.git'
            }
        }

    
    stage('Test Docker Credential') {
        steps {
            withCredentials([
                usernamePassword(
                    credentialsId: 'dockeer-crd',
                    usernameVariable: 'DOCKERHUB_USERNAME',
                    passwordVariable: 'DOCKERHUB_TOKEN'
                )
            ]) {
                bat '''
                    powershell -Command "$u=$env:DOCKERHUB_USERNAME; Write-Host ('Username length = ' + $u.Length); Write-Host ('Username first char = ' + $u.Substring(0,1))"
                    docker logout
                    echo %DOCKERHUB_TOKEN% | docker login -u %DOCKERHUB_USERNAME% --password-stdin
                '''
            }
        }
    }
    
    stage("Générer backend image "){
      steps {
        dir("formation_level_one/springboot/app"){
          bat "mvn clean install "
          bat "mvn clean package "
          bat "docker build -t nouran10/spring-app . --no-cache"
          bat "docker push nouran10/spring-app"
        }
      }
    }
    stage("Générer frontend image "){
      steps {
        dir("formation_level_one/angular-app"){
          bat "docker build -t nouran10/angular-app . --no-cache"
          bat "docker push nouran10/spring-app"
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
