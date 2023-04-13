pipeline { 
  agent any
  tools { 
    nodejs 'nodejs-19'
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
        post {
            failure {
                mail bcc: '', body: 'The test failed', cc: '', from: '', replyTo: '', subject: 'Filed Test', to: 'eric.munjiru@student.moringaschool.com'
            }
            success{
               mail bcc: '', body: 'The test was successful ', cc: '', from: '', replyTo: '', subject: 'Success Test', to: 'eric.munjiru@student.moringaschool.com' 
            }
        }
      steps { 
        sh 'npm test'
      }
    }
    stage('Deploy to Heroku') {
            steps {
                withCredentials([usernameColonPassword(credentialsId: 'Heroku1', variable: 'HEROKU_CREDENTIALS')]){
                sh 'git push https://${HEROKU_CREDENTIALS}@git.heroku.com/ericip.git master'
                }
            }
        
  
   }
 stage('slack notification'){
     steps{ 
         slackSend channel: '#jenkins', message: 'success'
     }
 }
  }
 
}
