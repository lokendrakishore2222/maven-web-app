pipeline {
    agent any

    tools {
        maven 'my-maven'
    }
    triggers {
        githubPush()
    }

    stages {

        stage('Checkout Code') {
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

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                      mvn verify \
                      -Dsonar.projectKey=my-project \
                      -Dsonar.projectName=my-project \
                      org.sonarsource.scanner.maven:sonar-maven-plugin:sonar
                    '''
                }
            }
        }

        stage('Upload to Nexus') {
            steps {
                sh 'mvn deploy -DskipTests'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sshagent(credentials: ['7da3c087-2825-4715-9f56-cb2c1d4a81b8']) {
                    sh '''
                      scp target/*.war \
                      ubuntu@18.213.227.245:/opt/tomcat/webapps/
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ CI/CD Pipeline completed successfully'

            emailext(
                subject: '✅ Build & Deployment Successful',
                body: """
                Hi Team,

                The Jenkins pipeline has completed successfully.

                Job: ${env.JOB_NAME}
                Build Number: ${env.BUILD_NUMBER}
                URL: ${env.BUILD_URL}

                Regards,
                Jenkins
                """,
                to: 'lokendrakishore2222@gmail.com, yuvakiran060@gmail.com'
            )
        }

        failure {
            echo '❌ CI/CD Pipeline failed'

            emailext(
                subject: '❌ Build or Deployment Failed',
                body: """
                Hi Team,

                The Jenkins pipeline has FAILED.

                Job: ${env.JOB_NAME}
                Build Number: ${env.BUILD_NUMBER}
                URL: ${env.BUILD_URL}

                Please check the logs.

                Regards,
                Jenkins
                """,
                to: 'lokendrakishore2222@gmail.com, yuvakiran060@gmail.com'
            )
        }
    }
}
