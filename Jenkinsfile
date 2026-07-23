pipeline {

    agent any

    tools {
        maven 'maven'
    }

    stages {

        stage('Build') {
            steps {
                echo "Building Project..."
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'sudo docker build -t nammastocks .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'credential',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {

                    sh '''
                    echo "$PASS" | sudo docker login -u "$USER" --password-stdin
                    '''
                }
            }
        }

        stage('Tag Image') {
            steps {
                sh 'sudo docker tag nammastocks mallikarjunb22/nammastocks:latest'
            }
        }

        stage('Push Image') {
            steps {
                sh 'sudo docker push mallikarjunb22/nammastocks:latest'
            }
        }

        stage('Logout') {
            steps {
                sh 'sudo docker logout'
            }
        }

        stage('Deploy') {

            steps {

                sh '''

                sudo docker stop nammastocks-container || true

                sudo docker rm nammastocks-container || true

                sudo docker run -d \
                --name nammastocks-container \
                -p 8085:8080 \
                -e DB_URL="$DB_URL" \
                -e DB_USERNAME="$DB_USERNAME" \
                -e DB_PASSWORD="$DB_PASSWORD" \
                mallikarjunb22/nammastocks:latest

                '''

            }

        }

    }

}
