pipeline {
    agent any 
    tools { 
        maven 'maven'
        nodejs 'node'
    }
    stages {
        stage ("Clean up"){
            steps {
                deleteDir()
            }
        }
        stage ("Clone repo"){
            steps {
                sh "git clone ht https://github.com/NouraneZouabi/formation_level_one.git"
            }
        }
        stage ("Generate frontend image") {
            steps {
                 dir("formation_level_one /angular-app"){
                    sh "docker build -t nouran10/angular-app-devops . --no-cache"
		    sh "docker push nouran10/angular-app-devops"
                }                
            }
        }
        stage ("Generate backend image") {
              steps {
                   dir("formation_level_one /springboot/app"){
                      sh "mvn clean install"
                      sh "docker build -t nouran10/spring-app-formation . --no-cache"
		      sh "docker push nouran10/spring-app-formation"
                  }                
              }
          }
        stage ("Run docker compose") {
            steps {
                 dir("formation_level_one "){
		    sh "docker compose down --volumes" 
		    sh "docker compose pull"
                    sh "docker compose up -d"
                }                
            }
        }
    }
}

