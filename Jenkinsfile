pipeline{
  agent any 
  stages{
    stage("Docker Cleanup"){
      steps{
        sh "docker stop $(docker ps -aq) || true"
        sh "docker rm $(docker ps -aq) || true"
      }
    }
    stage("Build Docker Images"){
      steps{
        sh "docker build -t flask-app-image ."
        sh "docker build -t nginx-image -f Dockerfilenginx"
      }
    }
    stage("Run Docker containers"){
      steps{
        sh "docker run -d --name flask-app flask-app-image"
        sh "docker run -d -p 80:80 --name nginx nginx-image"
      }
    }
  }
