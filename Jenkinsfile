pipeline {
    agent { label 'tuto' }
    
    enviroment {
        DOCKERHUB_CREDS = credentials('maxikey')
    }

    stages {
        stage('Build') {
            steps {
                container('podman') {
                    script {
                        sh 'podman build -t docker.io/maxibadaro/web-go:$BUILD_NUMBER -f Dockerfile'
                        sh 'podman login -u $DOCKERHUB_CREDS_USR -p DOCKERHUB_CREDS_PSW'
                        sh 'podman push docker.io/maxibadaro/web-go:$BUILD_NUMBER'
                    }
                }
                container('kubectl') {
                    script {
                        sh 'env'
                        sh 'kubectl get pods'
                    }
                }
                container('fortune') {
                    script {
                        sh 'fortune'
                    }
                }
            }
        }
    }
}
