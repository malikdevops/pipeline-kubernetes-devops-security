pipeline {
    agent {
        docker {
            image 'maven:3.6.3-jdk-11'
        }
    }
    stages {
        stage('Build Artifacts') {
            steps {
                sh "mvn clean package -DskipTests=true"
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Unit Test') {
            steps {
                sh "mvn test"
            }
        }
            post {
              always {
                junit 'target/surefire-reports/*.xml'
                jacoco.execPattern:'target/jacoco.exec'
            }
               }
  
    }
}


