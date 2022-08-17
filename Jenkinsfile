node {
    stage("Verify tooling") {
        sh '''
            docker version
            docker compose version
            docker info
        '''
    }

    stage("Clone repository data") {
        checkout scm
    }

    stage ("Logging to Docker Hub") {
        withCredentials([string(credentialsId: 'slyverstorm16-dockerhub-secret', variable: 'dockerHubPwd')]) {
            sh "docker login -u slyverstorm16 -p \"${dockerHubPwd}\""
        }
    }

    stage("Building docker images") {
        sh '''
            version=`cat package.json | grep -oP '(?<=\"version\": \")[^\"]*'`
            docker build -t slyverstorm16/devops-frontend:latest,$version .
        '''
    }

    stage("Push docker images") {
        sh '''
            version=`cat package.json | grep -oP '(?<=\"version\": \")[^\"]*'`
            docker push slyverstorm16/devops-frontend:latest,$version
        '''
    }

    stage("Start containers") {
        sh "docker-compose up -d"
    }
}