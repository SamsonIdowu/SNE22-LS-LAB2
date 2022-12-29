pipeline {
  agent any

  stages {

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

    stage('Test') {
      steps {
        sh 'docker run -d --name sne22-webapp -p 9000:9000 my-webapp'
        echo 'container test deployment was successfull>>>'
      }
    }

    stage('Push') {
      steps {
        echo 'now pushing working image to dockerhub...'
        sh 'docker login -u samsonidowu -p dckr_pat_qKDzQb7FvH9_keGP_gZCM4V1d_c docker.io'
        sh 'docker tag my-webapp samsonidowu/my-webapp:latest'
        sh 'docker push samsonidowu/my-webapp:latest'
        echo 'container image push was successfull>>>'
      }
    }
    stage('Cleanup') {
      steps {
        echo 'initializing test server cleanup...'
        sh 'docker rm -f $(docker ps -a -q)'
        echo 'removed container runtime'
        sh 'docker image prune'
        echo 'removed docker image'
      }
    }
    stage('Code-Analysis') {
      steps {
        echo'initializing the code analysis'
        sh 'apt update  -y'
        sh 'apt install npm -y'
        sh 'npm install snyk -g'
        snykSecurity severity: 'high', snykInstallation: 'Please define a Snyk installation in the Jenkins Global Tool Configuration. This task will not run without a Snyk installation.', snykTokenId: 'Snyk-Jenkins'
      }
    } 
  }
}
