pipeline {
  agent any

  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }

  environment {
    SONARQUBE_ENV = 'sq1'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Scan with SonarQube') {
      steps {
        withSonarQubeEnv(SONARQUBE_ENV) {
          sh './mvn clean org.sonarsource.scanner.maven:sonar-maven-plugin:3.8.1:sonar'
        }
      }
    }

    stage('Build') {
      steps {
        sh './mvn clean package'
      }
    }

    stage('Test') {
      steps {
        sh './mvn test'
      }
    }

    stage('Deploy') {
      steps {
        sh './mvn deploy'
      }
    }
  }

  post {
    success {
      echo 'Pipeline completed successfully!'
    }
    failure {
      echo 'Pipeline failed!'
    }
  }
}
