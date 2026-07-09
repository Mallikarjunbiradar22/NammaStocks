pipeline {
    agent any

    tools {
        jdk 'JDK21'
        maven 'Maven3'
    }

    stages {

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                sudo cp target/stock-market-0.0.1-SNAPSHOT.jar /opt/nammastocks/
                sudo systemctl restart nammastocks
                sudo systemctl status nammastocks --no-pager
                '''
            }
        }
    }
}