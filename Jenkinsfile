pipeline{
    agent any
  
    stages{
        stage('CLONE REPO') {
          steps {
              sh 'rm -rf /var/lib/jenkins/workspace/eline_multibranchpipeline-1_prod/docker-multibranch'
              sh 'git clone https://github.com/rakeshhh4/docker-multibranch.git '
          }
        }
        
        stage('BUILD DOCKER IMAGE') {
          steps {
                sh 'cd /var/lib/jenkins/workspace/eline_multibranchpipeline-1_prod/docker-multibranch'
                sh 'cp /var/lib/jenkins/workspace/eline_multibranchpipeline-1_prod/docker-multibranch/* /var/lib/jenkins/workspace/eline_multibranchpipeline-1_prod/'
                sh 'docker build -t rakesh2404/multi-pipelinetestprod:${BUILD_NUMBER} .'
            }
        }
        stage('PUSH IMAGE TO DOCKER-HUB') {
            steps {
                sh 'docker push rakesh2404/multi-pipelinetestprod:${BUILD_NUMBER}'
            }
        }
        stage('DEPLOY TO DOCKER HOST') {
            steps {
                sh 'docker -H tcp://172.31.81.249:2375 stop MYAPPMASTERPROD '
                sh 'docker -H tcp://172.31.81.249:2375 run --rm -itd --name MYAPPMASTERPROD --hostname MYAPPMASTERPROD -p 8000:80 rakesh2404/multi-pipelinetestprod:${BUILD_NUMBER}'
            }
                
        }
        stage('CHECK WEBAPP RECHABILITY') {
            steps {
                sh 'sleep 10s'
                sh 'curl http://3.88.60.108:10000'
            }
        }
    }
}
