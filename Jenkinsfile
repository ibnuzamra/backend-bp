node {

    stage("Git Clone"){

        git credentialsId: 'GITHUB_CREDENTIALS', url: 'https://github.com/ibnuzamra/backend-bp'
    }

    stage("Docker build"){
        sh 'docker version'
        sh 'docker build -t backend-prd .'
        sh 'docker image list'
        sh 'docker tag backend-prd ibnuzamra/backend-prd:latest'
    }

    withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD')]) {
        sh 'docker login -u ibnuzamra -p $PASSWORD'
    }

    stage("Push Image to Docker Hub"){
        sh 'docker push  ibnuzamra/backend-prd:latest'
    }
    
    stage("kubernetes deployment"){
        sh 'kubectl apply -f p-backend-deployment.yml'
    }
} 
