pipeline{
    agent any
    stages{
        stage("Code"){
            steps{
                echo "cloning the Code"
                git url: "https://github.com/hallar139/Notes-APP.git" , branch: "main"
            }
        }
        stage("Build"){
            steps{
                echo "Building the image"
                sh "docker build -t node-app-img ."
            }
        }
        stage("Push to Docker hub"){
            steps{
                echo "Pushing the image to Docker Hub"
                withCredentials([usernamePassword(credentialsId:"DockerHub", passwordVariable:"DockerHubPass", usernameVariable:"DockerHubUser")])
                {   
                    sh "docker tag node-app-img ${env.DockerHubUser}/node-app-img:latest"
                    sh "docker login -u ${env.DockerHubUser} -p ${env.DockerHubPass}"
                    sh "docker push ${env.DockerHubUser}/node-app-img:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploying the containers"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
