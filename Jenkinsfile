pipeline {
    agent any
     environment {
        registry = "[AWS ACCOUNT ID].dkr.ecr.us-east-1.amazonaws.com/rahulnk"
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
               withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws_cred', accessKeyVariable: '[value]
', secretKeyVariable: '(value)']]) {
    sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin [AWS ACCOUNT ID].dkr.ecr.us-east-1.amazonaws.com'
     sh 'docker push [AWS ACCOUNT ID].dkr.ecr.us-east-1.amazonaws.com/rahulnk:latest'
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
                sh 'docker run -d -p 8096:5000 --rm --name mypythonContainer [AWS ACCOUNT ID].dkr.ecr.us-east-1.amazonaws.com/rahulnk:latest'     
      }
    }
        }
    }
  }
