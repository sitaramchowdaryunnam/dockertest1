pipeline {
  environment { 
        registry = "riderd758/pipelinetestprod" 
        registryCredential = 'dockerhub' 
        dockerImage = '' 

    }
    agent any
    stages {

        stage('Clone Repo') {
          steps {
            sh 'rm -rf DockerTest1'
            sh 'git clone https://github.com/sitaramchowdaryunnam/DockerTest1.git'
            }
        }

        stage('Build Docker Image') {
          steps {
          sh 'cd /var/lib/jenkins/workspace/pipeline2'
	  sh 'cp  /var/lib/jenkins/workspace/pipeline2/DockerTest1/* /var/lib/jenkins/workspace/pipeline2'
          sh 'docker build -t riderd758/pipelinetestprod:${BUILD_NUMBER} .'
            }
        }

        stage('Push Image to Docker Hub') {
          steps {
           sh    'docker push riderd758/pipelinetestprod:${BUILD_NUMBER}'
           }
        }

        stage('Deploy to Docker Host') {
          steps {
            sh    'docker -H tcp://10.1.2.100:2375 stop prodwebapp1 || true'
            sh    'docker -H tcp://10.1.2.100:2375 run --rm -dit --name prodwebapp1 --hostname prodwebapp1 -p 8000:80 riderd758/pipelinetestprod:${BUILD_NUMBER}'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://10.1.2.100:8000'
          }
        }

    }
}
