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
    stage("Docker hub login"){
      steps {
        withCredentials([
          usernamePassword(
            credentialsId: 'dockercrd',
            usernameVariable: 'DOCKERHUB_USERNAME',
            passwordVariable: 'DOCKERHUB_TOKEN')
        ])
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
