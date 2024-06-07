pipeline{
  agent any 
  stages{
    stage("Docker Cleanup"){
      steps{
        sh "docker stop \$(docker ps -aq) || true"
        sh "docker rm \$(docker ps -aq) || true"
        sh "docker network create newnetwork || true"
      }
    }
    stage("Build Docker Images"){
      steps{
        sh "docker build -t flask-app-image ."
        sh "docker build -t nginx-image -f Dockerfilenginx ."
      }
    }
    stage("Run Docker containers"){
      steps{
        sh "docker run -d --network newnetwork --name flask-app flask-app-image"
        sh "docker run -d --network newnetwork -p 80:80 --name nginx nginx-image"
      }
    }
    stage("Run trivy security scan"){
      steps{
        sh "trivy fs -f json -o scanresults.json ."
      }
    }
    stage("Run unit tests"){
      steps{
        sh "/usr/bin/python3 test.py"
      }
    }
  }
  post{
    always{
      archiveArtifacts artifacts: 'scanresults.json', allowEmptyArchive: true
    }
  }
}
