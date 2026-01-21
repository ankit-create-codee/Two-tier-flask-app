pipeline {
    agent any
    stages {
      stage("Code")
      {
          steps{
            git branch: 'main', url: 'https://github.com/ankit-create-codee/Two-tier-flask-app.git'

          }
      }
      stage("Build")
      {
          steps{
              sh "docker build -t flask-app ."
          }
      }
      stage("Test")
      {
         steps{
             echo "testing in prog"
         } 
      }
      stage("Push to DockerHub")
      {
       steps{
           withCredentials([usernamePassword(
               credentialsId:"creds",
                passwordVariable:"pass",
                usernameVariable: "user")]){
           sh "docker login -u ${env.user} -p ${env.pass}"
           sh "docker image tag flask-app ${env.user}/flask-app:latest"
           sh "docker push  ${env.user}/flask-app:latest"
           }
       }
      }
      stage("Deploy")
      {
          steps{
           sh "docker compose up -d --build"
          }
      }
    }
}
post{
        success{
            script{
                emailext from: 'ankujain128@gmail.com',
                to: 'ankujain128@gmail.com',
                body: 'Build success for Demo CICD App',
                subject: 'Build success for Demo CICD App'
            }
        }
        failure{
            script{
                emailext from: 'ankujain128@gmail.com',
                to: 'ankujain128@gmail.com',
                body: 'Build Failed for Demo CICD App',
                subject: 'Build Failed for Demo CICD App'
            }
        }
    }

