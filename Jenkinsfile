pipeline {
  agent any

  stages {

    // Building, Testing and the Application.
    stage('Install') {
      steps {
        sh 'sudo apt-get update'
        sh 'sudo apt install docker.io -y && sudo snap install docker'
        echo 'necessary updates and installation completed>>>'
      }
    }

    stage('Build') {
      steps {
        sh 'docker rm -f sne22-webapp'
        sh 'docker build -t my-webapp .'
        echo 'build completed>>>'
      }
    }

    // Testing the container run-time
    stage('Test') {
      steps {
        sh 'docker run -d --name sne22-webapp -p 9000:9000 my-webapp'
        echo 'container test deployment was successfull>>>'
      }
    }

    // Pushing application and it's artifacts to container repository
    stage('Push') {
      steps {
        echo 'now pushing working image to dockerhub...'
        sh 'docker login -u samsonidowu -p dckr_pat_qKDzQb7FvH9_keGP_gZCM4V1d_c docker.io'
        sh 'docker tag my-webapp samsonidowu/my-webapp:latest'
        sh 'docker push samsonidowu/my-webapp:latest'
        echo 'container image push was successfull>>>'
      }
    }

    // Deleting the container from the jenkins agent node
    stage('Cleanup') {
      steps {
        echo 'initializing test server cleanup...'
        sh 'docker rm -f $(docker ps -a -q)'
        echo 'removed container runtime'
        sh 'docker image prune'
        echo 'removed docker image'
      }
    }
  }

  // Deploying the container to production environment (webserver machine)
  agent {label 'webserver'}

  stages {
    stage('Deploy to Production') {
      steps {
        echo 'deploying to production server>>>'
        sh 'sudo apt-get update'
        sh 'sudo apt install docker.io -y && sudo snap install docker'
        sh 'docker pull samsonidowu/my-webapp:latest'
        sh 'docker run -d --name sne22-webapp -p 9000:9000 my-webapp'
        echo 'container was successfully deployed to production!!!'
        echo 'you can now access the application on 10.1.1.40:9000 >>>'
      }
    }
  }
}
