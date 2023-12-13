pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        
        sh 'docker build -t my-flask .'
        sh 'docker tag my-flask $dockertag'
      }
    }
    stage('Test') {
      steps {
        sh 'docker run -d -it --name appserver2 -p3003:5000 my-flask'
      }
    }
    stage('Deploy') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerpass', passwordVariable: 'dpassword', usernameVariable: 'dusername')]) {
          sh "docker login -u $dusername -p $dpassword"
          sh 'docker push $dockertag'
        }
      }
    }
    
  }

post{
    always {
        script {
            mail to: 'aws.vjy@gmail.com', subject: 'Jenkins', body: 'Job From Jenkins'
        }
    }
}
                    
}

