pipeline {
  agent any

  stages {
    docker {
            image 'maven:3.6.3-jdk-11'
        }
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' //so that they can be downloaded later
            }
        }   
    }
}