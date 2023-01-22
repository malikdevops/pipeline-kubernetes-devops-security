pipeline {
    agent any
    stages {
        stage('Build Artifacts') {
            steps {
                sh "mvn clean package -DskipTests=true"
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

   stage('Unit Tests - JUnit and JaCoCo') {
      steps {
        sh "mvn test"
      }
    }
    stage('Security Test') {
        steps {
            sh 'mvn verify -Psecurity'
        }
    }

    stage('Mutation Tests - PIT') {
      steps {
        sh "mvn org.pitest:pitest-maven:mutationCoverage"
      }
    }
    stage('Integration Tests - Cucumber') {
      steps {
        sh "mvn verify -Pintegration"
      }
        
    }
    stage('Performance Tests - JMeter') {
      steps {
        sh "mvn verify -Pperformance"
      }
    }
    stage('Static Analysis - Checkstyle') {
      steps {
        sh "mvn verify -Pcheckstyle"
      }
    }
    stage('Static Analysis - PMD') {
      steps {
        sh "mvn verify -Ppmd"
      }
    }
    stage('Static Analysis - FindBugs') {
      steps {
        sh "mvn verify -Pfindbugs"
      }
    }


       stage('SonarQube - SAST') {
      steps {
        withSonarQubeEnv('SonarQube') {
          sh "mvn sonar:sonar \
		              -Dsonar.projectKey=numeric-application \
		              -Dsonar.host.url=http://jenkins-adm.eastus.cloudapp.azure.com:9000"
        }
        timeout(time: 2, unit: 'MINUTES') {
          script {
            waitForQualityGate abortPipeline: true
          }
        }
      }
    }

    stage('Static Analysis - OWASP Dependency Check') {
      steps {
        sh "mvn verify -Powasp"
      }
    }

    stage('Static Analysis - OWASP ZAP') {
      steps {
        sh "mvn verify -Powasp-zap"
      }
    }  

    }
}
