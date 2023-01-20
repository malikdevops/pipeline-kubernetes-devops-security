// This is a sample Jenkinsfile you can use to deploy a Java app.
pipeline {
    agent {
        docker {
            image 'maven:3.6.3-jdk-11'
        }
    stages {
        stage('Build Artifact') {
            steps {
                sh "mvn clean package -DskipTests=true"
                archive 'target/*.jar' //so that they can be downloaded later'
            }
        }
        
    }

    }
    
}
