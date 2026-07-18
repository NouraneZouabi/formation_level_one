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
    
    stage("Générer backend image "){
      steps {
        dir("formation_level_one/springboot/app"){
          sh "mvn clean install "
          sh "mvn clean package "
          sh "sudo docker build -t nouran10/spring-app . --no-cache"
          sh "sudo docker push nouran10/spring-app"
        }
      }
    }
    stage("Générer frontend image "){
      steps {
        dir("formation_level_one/angular-app"){
          sh "sudo docker build -t nouran10/angular-app . --no-cache"
          sh "sudo docker push nouran10/spring-app"
        }
      }
    }

    stage("Lancement du docker compose "){
      steps {
        dir("formation_level_one/"){
          sh "sudo docker compose down --volumes "
          sh "sudo docker compose pull"
          sh "sudo docker compose up -d "
        }
      }
  }  
  }
}
