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
      parallel {
        stage('Package Code') {
          steps {
            sh 'tar -czvf node.ta.gz *'
          }
        }

        stage('Notify Slack') {
          steps {
            slackSend(color: '#3EA652', attachments: 'BlueOcen', blocks: 'sssss')
          }
        }

      }
    }

    stage('Publish') {
      steps {
        archiveArtifacts 'node.tar.jz'
        cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenSuccess: true, cleanWhenUnstable: true)
      }
    }

  }
}