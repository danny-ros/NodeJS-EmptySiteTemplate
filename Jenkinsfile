pipeline {
  agent {
    node {
      label 'CentOS7'
    }

  }
  stages {
    stage('Check Out Code') {
      steps {
        git(url: 'https://github.com/danny-ros/NodeJS-EmptySiteTemplate.git', branch: 'master', changelog: true, poll: true)
      }
    }

    stage('Build') {
      steps {
        sh 'npm install'
      }
    }

    stage('Test Code') {
      steps {
        sh '''node server.js &
              sleep 5 &&
              curl localhost:8090
              if [[ "x$?" == "x0" ]];
              then    echo good;
              else exit 1;
              fi'''
      }
    }

    stage('Package Code') {
      steps {
        sh 'tar -czvf node.tar.gz *'
      }
    }

    stage('Publish the Archive') {
      parallel {
        stage('Publish the Archive') {
          steps {
            archiveArtifacts 'node.tar.gz'
            cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenSuccess: true, cleanWhenUnstable: true)
          }
        }

        stage('Slack') {
          steps {
            slackSend(message: '"${env.JOB_NAME} #${env.BUILD_NUMBER} - Started By ${env.BUILD_USER} (${env.BUILD_URL})"', color: '#3EA652', channel: 'danny-ros')
          }
        }

      }
    }

    stage('Slack Send') {
      steps {
        slackSend(channel: 'danny-ros', color: '#3EA652', message: "${env.JOB_NAME} #${env.BUILD_NUMBER} - Started By ${env.BUILD_USER} (${env.BUILD_URL})")
      }
    }

  }
}