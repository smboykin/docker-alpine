// Jenkinsfile to use for ARCH workflow
node {
    def targetRegistry = env.DOCKER_STAGING_REGISTRY ?: 'docker-staging-local.chip-repo.childrens.harvard.edu'
    def imageName = env.ALPINE_BASE_IMAGE_NAME ?: 'alpine'
    def image
    def imageMajorVersion = env.ALPINE_VERSION ?: '3.7'
    stage('Clone repository') {
        checkout scm
    }
    stage('Build and tag image') {
        // Use the provided build script
        sh "./build versions/library-${imageMajorVersion}/x86_64/options"
        image = docker.image(imageName);

    }
    stage('Push image') {
        docker.withRegistry("https://${targetRegistry}", 'jenkins-chip-repo') {
            image.push()
            image.push("${imageMajorVersion}")
        }
    }
}