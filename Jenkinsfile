pipeline {
  agent any
  stages {
    stage('Compile') {
      steps {
        sh './mvnw clean compile'
      }
    }

    stage('Static Analysis') {
      steps {
        sh '''./mvnw sonar:sonar \\
  -Dsonar.projectKey=Petclinic \\
  -Dsonar.host.url=http://172.31.28.235:9000 \\
  -Dsonar.login=sqp_0a731687465839d4c15cf47fa29a524e33f87f38'''
      }
    }

  }
}