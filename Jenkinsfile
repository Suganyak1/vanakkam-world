pipeline {
    agent any
    environment {
        IMAGE_NAME = "suganyak12/java-app"
        IMAGE_TAG = "1.0"
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Suganyak1/vanakkam-world.git'
            }
        }


        stage('Build with Maven') {
            steps {
                withMaven(maven: 'Maven-New') {
                    sh 'mvn clean package'
                }
            }
        }


        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }


        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push $IMAGE_NAME:$IMAGE_TAG
                    '''
                }
            }
        }


        stage('Run Container') {
            steps {
                sh 'docker run -d --name myapp -p 81:8080 $IMAGE_NAME:$IMAGE_TAG'
            }
        }
    }


    post {
        always {
            echo "Pipeline Finished!!!!!!!!!!!!!!!!!!!!"
        }
    }
}
