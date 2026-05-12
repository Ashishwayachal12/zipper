pipeline {
    agent any
    environment {
        IMAGE_NAME = "ashishwayachal12/zipper"
        IMAGE_TAG  = "${BUILD_ID}"
        SERVER_IP  = "16.171.196.70"
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
                withDockerRegistry(credentialsId: '83b1b4c6-56ef-4aa3-ab6b-dc6bb7dc6851')   {
                sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
            }
        }
    }
    post {

    success {
        echo "DEPLOYMENT SUCCESSFUL"
        mail to: 'ashishwayachal14@gmail.com,kabshataiyub@gmail.com,siddardha070@gmail.com',
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
        mail to: 'ashishwayachal14@gmail.com,kabshataiyub@gmail.com,siddardha070@gmail.com',
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
