pipeline {
  agent any

    
  stages {
    stage("clean up"){
      steps {
        deleteDir()
      }
    }
    
    stage("clone repo"){
      steps {
        sh "git clone https://github.com/NouraneZouabi/formation_level_one.git"
      }
    }

    stage("Docker hub login"){
      steps {
        withCredentials([
          usernamePassword(
            credentialsId: 'dockercrd',
            usernameVariable: 'DOCKERHUB_USERNAME,
            passwordVariable: 'DOCKERHUB_TOKEN')
        ])
        {
          sh '''
            echo "$DOCKERHUB_TOKEN" | docker login \
                -u "$DOCKERHUB_USERNAME" \
                --password-stdin
              '''
        }
      }
    }
    
    stage("Générer backend image "){
      steps {
        dir("formation_level_one/springboot/app"){
          sh "mvn clean install "
          sh "mvn clean package "
          sh "docker build -t nouran10/spring-app . --no-cache"
          sh "docker push nouran10/spring-app"
        }
      }
    }
    stage("Générer frontend image "){
      steps {
        dir("formation_level_one/angular-app"){
          sh "docker build -t nouran10/angular-app . --no-cache"
          sh "docker push nouran10/spring-app"
        }
      }
    }

    stage("Lancement du docker compose "){
      steps {
        dir("formation_level_one/"){
          sh "docker compose down --volumes "
          sh "docker compose pull"
          sh "docker compose up -d "
        }
      }
  }  
  }
}
