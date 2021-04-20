pipeline {
    agent any 
    environment {
        DOCKER_IMAGE = "docker-test-server-1234"
        CONTAINER_NAME = "server-test-011"
    }
    stages {
        stage ('Clone Repository') {
            steps {
                echo "Checkout Git Repo"
               // git credentialsId: 'github', url: 'https://github.com/radhakrishna4687/:${GITHUB_BRANCH}'
                git 'https://github.com/radhakrishna4687/java-webapp.git'
                echo "Complted the Checkout the Git"
            }

        }
        stage ('Build Docker Image') {
            steps {
                sh '''
                    docker build . -t ${DOCKER_IMAGE}
                    docker images
                ''' 
                //sh "docker build .  -t radhakrishna4687/test-image-1:${DOCKER_TAG} "
            }
        }
        stage ('Run the Docker Container') {
            steps {
                sh '''
                    docker run -itd --name=${CONTAINER_NAME} -p 8091:8080 ${DOCKER_IMAGE}
                    docker ps 
                    docker inspect ${CONTAINER_NAME}
                '''    
            }
        }
        stage ('Docker image push to Dockerhub Repository') {
            steps {
                sh 'docker tag ${DOCKER_IMAGE} radhakrishna4687/krishna-web-app:v2'
            }
        }
        stage ('Push docker image to DockerHub') {
            steps {
                withDockerRegistry([ credentialsId: "Dockerhub", url: "" ]) {
                //withCredentials([string(credentialsId: 'dockerhubcred', variable: 'dockerhubpwd')]) {
                //withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhubpwd')])
                    //sh 'sudo su -p root'
                   // sh 'docker login --username radhakrishna4687  --password-stdin ${dockerhubpwd}'
                    sh 'docker push radhakrishna4687/krishna-web-app:v2'
                }
            }
        }
        stage ('Docker remove contaner and image') {
            steps {
                sh '''
                    docker stop ${CONTAINER_NAME} 
                    docker rm ${CONTAINER_NAME} 
                    docker rmi ${DOCKER_IMAGE}
                '''    
            }
        }
    }
}
//Dynamically allocate the version name with latest commit id
//def getDockerTag() {
//    def tag = sh script: 'git rev-parse HEAD', returnStdout: true
    //return tag
//}