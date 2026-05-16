pipeline {
    agent any
    environment {
        IMAGE_NAME = "ashishwayachal12/zipper"
        IMAGE_TAG  = "${BUILD_ID}"
        SERVER_IP  = "13.53.245.207"
        APP_PORT   = "1111"
    }

    stages {
        stage("CODE") {
            steps {
                echo "Cloning the code..."
                git url: "https://github.com/Ashishwayachal12/zipper.git", branch: "main"
                echo "Code cloning successful"
            }
        }

        stage("BUILD") {
            steps {
                echo "Building Docker image..."
                sh "docker build -t zipper:${IMAGE_TAG} ."
            }
        }
        stage("DEPLOY") {
            steps {
                echo "Deploying container..."
                sh '''docker compose up -d --build
                     docker ps -a'''
                
            }
        }
        stage("TAG") {
            steps {
                echo "Creating Docker Hub tag..."
                sh "docker tag zipper:${IMAGE_TAG} ${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }
            stage("PUSH") {
            steps {
                script{
                echo "Pushing image to Docker Hub..."
                withDockerRegistry(credentialsId: '8d72d5f5-1a1d-4a72-aec3-4d20f34b4238') {
                sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
            }
        }
    }
    post {

    success {
        echo "DEPLOYMENT SUCCESSFUL"
        mail to: 'ashishwayachal14@gmail.com,kabshataiyub@gmail.com',
        subject: "SUCCESS: Jenkins Build ${BUILD_NUMBER}",
        body: """
        Build Status : SUCCESS
        Job Name : ${JOB_NAME}
        Build Number : ${BUILD_NUMBER}
        Application URL:
        http://${SERVER_IP}:${APP_PORT}
        """
    }

    failure {
        echo "DEPLOYMENT FAILED"
        mail to: 'ashishwayachal14@gmail.com,kabshataiyub@gmail.com',
        subject: "FAILED: Jenkins Build ${BUILD_NUMBER}",
        body: """
        Build Status : FAILED
        Job Name : ${JOB_NAME}
        Build Number : ${BUILD_NUMBER}
        Check Console Output:
        ${BUILD_URL}
        Server IP : ${SERVER_IP}
        Port : ${APP_PORT}
        """
    }
  }
}
