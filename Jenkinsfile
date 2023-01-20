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
                archive 'target/*.jar' 
            }
        }

        stage('Unit Test') {
            steps {
                sh "mvn test"
            }
        }

    }
}

