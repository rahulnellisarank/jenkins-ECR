pipeline {
    agent any
     environment {
        registry = "749897135281.dkr.ecr.us-east-1.amazonaws.com/rahulnk"
    }
   
    stages {
          stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/rahulnellisarank/jenkins-ECR.git'
            }
        }
           stage('Building image') {
             steps{
                  script {
                   dockerImage = docker.build registry
                   }
      }
           }
    
            stage('Pushing to ECR') {
             steps{  
                  script {
               withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws_cred', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
    sh 'aws ecr get-login-password --region us-west-1 | docker login --username AWS --password-stdin 759513982256.dkr.ecr.us-west-1.amazonaws.com'
     sh 'docker push 759513982256.dkr.ecr.us-west-1.amazonaws.com/practice:latest'
}

}
                  }
            }
             stage('stop previous containers') {
               steps {
            sh 'docker ps -f name=mypythonContainer -q | xargs --no-run-if-empty docker container stop'
            sh 'docker container ls -a -fname=mypythonContainer -q | xargs -r docker container rm'
         }
       }
            stage('Docker Run') {
              steps{
                   script {
                sh 'docker run -d -p 8096:5000 --rm --name mypythonContainer 759513982256.dkr.ecr.us-west-1.amazonaws.com/practice:latest'     
      }
    }
        }
    }
  }
