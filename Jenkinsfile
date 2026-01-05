node {
    def mavenHome = tool name : "maven3.9.12"
    stage('CheckoutGit') {
        git credentialsId: 'be6df469-826f-4f88-b203-35d6478478e9',
            url: 'https://github.com/lokendrakishore2222/maven-web-app.git'
    }

    stage('Build') {
        sh '${avenHome}/bin/mvn clean package'
    }
}
