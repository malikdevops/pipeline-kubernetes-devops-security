pipeline {
    agent any
    stages {
        stage('Build Artifacts') {
            steps {
                sh "mvn clean package -DskipTests=true"
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }



        stage('Unit Test') {
            steps {
                sh 'mvn test'
                script {
                    junit 'target/surefire-reports/*.xml'
                }
            }
            post {
                success {
                    jacoco coverage: 'target/jacoco.exec'
                }
            }
        }

        stage('Security Test') {
            steps {
                sh 'mvn verify -Psecurity'
            }
        }


        
    }
}
