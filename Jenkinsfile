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
        script {
          def retries = 5
          def wait = 10
          for (int i = 0; i < retries; i++) {
            def result = sh(script: 'curl -f http://localhost:8081', returnStatus: true)
            if (result == 0) {
              echo "Servicio disponible"
              break
            } else {
              echo "Intento ${i + 1} fallido, esperando ${wait}s..."
              sleep wait
            }
          }
        }
      }
      post {
        failure {
          sh 'docker logs c-msvc-webflux-producto || true'
        }
      }

    }
  }
}
