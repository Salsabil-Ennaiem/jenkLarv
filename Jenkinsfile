pipeline {
    agent any

    // Optional: define your build args here if they are fixed
    environment {
        // Example args - change or remove as needed
        APP_VERSION = "1.0.${BUILD_NUMBER}"
        SOME_ARG    = "production"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // This is the most flexible way - works no matter what your Dockerfile is called
                    def dockerfile = 'Dockerfile'  // default name
                    def dockerfilePath = './'      // default location

                    // If you have a file named something else (e.g. Dockerfile.dev, docker/Dockerfile, etc.)
                    // just change the two lines above, or detect it automatically:
                    
                    // Auto-detect any file named Dockerfile* in the workspace root
                    def dockerfileList = findFiles(glob: '**/Dockerfile*')
                    if (dockerfileList.size() > 0) {
                        dockerfile = dockerfileList[0].name
                        dockerfilePath = dockerfileList[0].path.replace(dockerfileList[0].name, '')
                        echo "Found Dockerfile: ${dockerfile} in ${dockerfilePath}"
                    } else {
                        error "No Dockerfile found in the workspace!"
                    }

                    // Build the image with as many build-args as you want
                    def image = docker.build(
                        "myapp:${env.BUILD_NUMBER}",  // image name:tag  (change "myapp" to your repo/name)
                        // Docker build options
                        "-f ${dockerfilePath}${dockerfile} " +               // <-- specify exact Dockerfile
                        "--build-arg APP_VERSION=${env.APP_VERSION} " +      // <-- example arg 1
                        "--build-arg SOME_ARG=${env.SOME_ARG} " +            // <-- example arg 2
                        "--build-arg MORE_ARG=whatever " +                   // <-- you can add as many as you want
                        "."                                                  // build context (current dir)
                    )

                    // Optional: push it somewhere
                    // docker.withRegistry('https://registry.example.com', 'credentials-id') {
                    //     image.push()
                    //     image.push('latest')
                    // }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}