pipeline { 
  agent any
  tools { 
    NodeJS "NodeJs 19.8.1"
  }
  stages { 
    stage('clone repository') {
      steps { 
        git 'https://github.com/eric-lewis94/gallery.git'
      }
    }
    stage('Build the project') {
      steps { 
        sh 'npm install'
      }
    }
    stage('Tests') {
      steps { 
        sh 'npm test'
      }
    }
    stage('Email notification') {
      steps { 
        sh 'emailext body: 'Test failed', subject: 'Error', to: 'eric.munjiru@student.moringaschool.com'
      }
    }
    stage('Email notification') {
      steps { 
        sh 'emailext body: 'Test failed', subject: 'Error', to: 'eric.munjiru@student.moringaschool.com'
      }
    }
  }
}
