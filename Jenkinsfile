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

 
  }
}
