pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          - name: git
            image: alpine/git:latest
            command:
            - cat
            tty: true
          - name: docker
            image: docker:latest
            command:
            - cat
            tty: true
            volumeMounts:
             - mountPath: /var/run/docker.sock
               name: docker-sock
          volumes:
          - name: docker-sock
            hostPath:
              path: /var/run/docker.sock    
        '''
    }
  }
  stages {
    stage('Checkout') {
      steps {
        // Checkout your source code baha
        checkout scm
     }
  }
  stage('Build Docker Image') {
      steps {
        container('docker') {
          script {
            // Build the Docker image
            def appImage = docker.build --no-cache=true ("bahaben/dolibarr:${env.BUILD_NUMBER}", "-f Dockerfile .")
            // Push the Docker image to your Docker registry
            docker.withRegistry('', '10') {
              appImage.push()
            }
          }
        }
      }
    }
  }
}
