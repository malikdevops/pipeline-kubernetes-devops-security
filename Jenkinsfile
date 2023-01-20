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

        stage('Test') {
            steps {
                sh 'mvn test -Dmaven.test.failure.ignore=true -Dmaven.javadoc.skip=true -Djacoco.skip=false'
                junit 'target/surefire-reports/*.xml'
          }
            post {
                always {
                    jacoco 'target/jacoco.exec'
                }
          }
        }


        


    }
}


