pipeline {
  agent any
  stages {
    stage('Check Out Code') {
      steps {
        git(url: 'https://github.com/danny-ros/NodeJS-EmptySiteTemplate.git', branch: '/master', changelog: true, poll: true)
      }
    }

  }
}