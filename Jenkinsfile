pipeline {
  agent {
    node {
      label 'test'
    }

  }
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
  -Dsonar.host.url=http://172.31.28.235:9000/ \\
  -Dsonar.login=sqp_2044a0b232cc85f43c75d83115e1717a37c867de'''
      }
    }

    stage('Unit Test') {
      steps {
        sh './mvnw "-Dtest=**/petclinic/*/*.java" test'
      }
    }

    stage('Package') {
      steps {
        sh '''./mvnw package -DskipTests=true
'''
      }
    }

    stage('Deploy') {
      parallel {
        stage('Deploy') {
          steps {
            sh '''./mvnw spring-boot:run </dev/null &>/dev/null &
'''
          }
        }

        stage('Integration and Performance Tests') {
          steps {
            sh './mvnw verify'
            junit '**target/surefire-reports/*.xml'
            perfReport '**/target/jmeter/results/*'
          }
        }

      }
    }

  }
}