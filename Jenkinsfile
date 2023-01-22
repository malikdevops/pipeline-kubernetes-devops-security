pipeline {
    environment {
        IMAGE_NAME = "numeric-webapp"
        APP_CONTAINER_PORT = "8080"
        DOCKERHUB_ID = "malikdevops97"
        DOCKERHUB_PASSWORD = credentials('dockerhub_password')
        IMAGE_TAG = "latest"
    }
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

    stage('Mutation Tests - PIT') {
      steps {
        sh "mvn org.pitest:pitest-maven:mutationCoverage"
      }
    }
    stage('Build image') {
           agent any
           steps {
              script {
                sh 'docker build  -t ${DOCKERHUB_ID}/$IMAGE_NAME:$IMAGE_TAG .'
                
              }
           }
       }

       stage('Run container based on builded image') {
          agent any
          steps {
            script {
              sh '''
                  echo "Cleaning existing container if exist"
                  docker ps -a | grep -i $IMAGE_NAME && docker rm -f ${IMAGE_NAME}
                  docker run --name ${IMAGE_NAME} -d -p 8010:$APP_CONTAINER_PORT  ${DOCKERHUB_ID}/$IMAGE_NAME:$IMAGE_TAG
                  sleep 5
              '''
             }
          }
       }

        stage('Test') {
            steps {
            script {
                sh '''
                echo "Testing the application"
                curl -s http://localhost:8010/ | grep -i "200"
                '''
            }
            }
        }

         stage('Clean container') {
          agent any
          steps {
             script {
               sh '''
                   docker stop $IMAGE_NAME
                   docker rm $IMAGE_NAME
               '''
             }
          }
        }


    }
}
