pipeline {
    agent { label 'tuto' }
    
    environment {
        DOCKERHUB_CREDS = credentials('maxikey')
    }

    stages {

        stage('Build') {
            when {
                branch 'develop'
            }
            steps {
                container('podman') {
                    script {
                        sh 'podman build -t docker.io/maxibadaro/web-go:$BUILD_NUMBER -f Dockerfile'
                        sh 'podman login docker.io -u $DOCKERHUB_CREDS_USR -p $DOCKERHUB_CREDS_PSW'
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
        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                container('kubectl') {
                    script {
                        sh 'kubectl apply -f todo.yaml'
                        sh 'kubectl rollout restart deployment -n maxi web-go'
                        sh 'kubectl rollout status deployment -n maxi web-go'
                    }
                }
            }
        }
    }
}
