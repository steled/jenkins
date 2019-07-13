node {
    def app
    def source
    stage('Build Docker Image') {
        source = checkout(scm)
        app = docker.build("steled/sample-app:${env.BUILD_ID}", "--label steled.sample.commit=${source.GIT_COMMIT} .")
    }

    stage('Publish to Docker Hub') {
        docker.withRegistry("https://index.docker.io/v1/", "dockerhub") {
            app.push(env.BUILD_ID)
        }
    }

    stage('Deploy to Production') {
        docker.withServer('tcp://production:2376', 'production') {
            sh "docker service update --image steled/sample-app:${env.BUILD_ID} sample"
        }
    }
}
