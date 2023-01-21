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
                sh "mvn test"
            }
            post {
                always {
                    script {
                        junit skipPublishingChecks: true 'target/surefire-reports/*.xml'
                        jacoco.execPattern 'target/jacoco.exec'
                    }
                }
            }
        }
    }
}
