pipeline {
  agent any

  stages {
    stage('Clonar servicios') {
      steps {
        // Jenkins ya clonó el repo, pero este paso es opcional si tienes configurado SCM
        echo 'Repositorio clonado'
      }
    }
    
    stage('Descargar imágenes') {
      steps {
        sh 'docker-compose pull'
      }
    }

    stage('Levantar contenedores con Docker Compose') {
      steps {
        sh 'docker-compose down --remove-orphans'
        sh 'docker-compose up -d'
      }
    }

    stage('Verificar servicios') {
      steps {
        sh 'docker ps'
      }
    }

    stage('Pruebas de conectividad') {
      steps {
        sh 'curl -f http://localhost:8081 || exit 1'
      }
    }
  }
}
