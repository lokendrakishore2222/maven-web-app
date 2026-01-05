pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    stages {

        stage('Git Clone') {
            steps {
                git credentialsId: 'be6df469-826f-4f88-b203-35d6478478e9',
                    url: 'https://github.com/lokendrakishore2222/maven-web-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

    }

    post {
        success {
            echo 'Build completed successfully'
        }
        failure {
            echo 'Build failed'
        }
    }
}
