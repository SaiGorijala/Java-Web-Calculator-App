pipeline {
  agent any

  environment {
    SONAR_TOKEN = credentials('sonar-token')
    NEXUS_USER  = credentials('nexus-user')
    NEXUS_PASS  = credentials('nexus-password')
  }

  stages {

    stage('Checkout') {
      steps {
        git branch: 'main',
            url: 'https://github.com/SaiGorijala/Java-Web-Calculator-App.git'
      }
    }

    stage('Build') {
      steps {
        sh 'mvn clean package -DskipTests'
      }
    }

    stage('SonarQube Analysis') {
      steps {
        sh """
          mvn sonar:sonar \
          -Dsonar.projectKey=java-web-calculator \
          -Dsonar.projectName=Java-Web-Calculator \
          -Dsonar.host.url=http://sonarqube-server:9000 \
          -Dsonar.login=$SONAR_TOKEN
        """
      }
    }

    stage('Publish to Nexus') {
      steps {
        sh """
          mvn deploy \
          -DskipTests \
          -Dnexus.username=$NEXUS_USER \
          -Dnexus.password=$NEXUS_PASS
        """
      }
    }
  }
}