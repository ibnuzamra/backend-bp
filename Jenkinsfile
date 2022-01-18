pipeline{
    agent any
    stages {
        stage('Build Backend') {
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: '1', url: 'https://github.com/ibnuzamra/backend-bp.git']]])
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                  sh 'docker build -t ibnuzamra/backend-prd:latest .'
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
                    sh 'docker login -u ibnuzamra -p ${dockerhubpwd}'
                 }  
                 sh 'docker push ibnuzamra/backend-prd:latest'
                }
            }
        }
    
    stage('Deploy App on k8s') {
      steps {
            sshagent(['cilist-cluster']) {
            sh "rsync -chavzP p-backend-deployment.yml ubuntu@13.250.114.157:/home/ubuntu/"
            script {
                sh "ssh ubuntu@13.250.114.157 kubectl delete -f p-backend-deployment.yml"
            }
            script {
                try{
                    sh "ssh ubuntu@13.250.114.157 kubectl create -f p-backend-deployment.yml"
                }catch(error){
                    sh "ssh ubuntu@13.250.114.157 kubectl create -f p-backend-deployment.yml"
                }
            }
}
        }
    }
    
    }
}