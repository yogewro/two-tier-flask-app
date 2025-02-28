pipeline{
    
    agent {label "dev" };
    
    stages{
        stage("Code"){
            steps{
                echo "code Clone form Github"
                git url :"https://github.com/yogewro/two-tier-flask-app.git",branch: "master"
                echo "git clone ho gaya hoyeeee"
            }
        }
        stage("Build"){
            steps{
                echo "image build for docker "
                sh "docker build -t two-tier-flask-app ."
            }
            
        }
        stage("Test"){
            steps{
                echo "test ho gaye"
                 
            }
            
        }
        stage("Push to Dokcer Hub"){
            steps{
                withCredentials([usernamePassword(
                credentialsId:"dockerhub",
                passwordVariable:"dockerHubPass",
                usernameVariable:"dockerHubUser"
                )]){
                    
                 "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app:latest"
                sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                }
            }
        }
        stage("deploy"){
            steps{
                
                sh "docker compose up -d --build flask-app"
            }
            
        }
         
    }
post{
    success {
            script {
                emailext from: 'yogewro@gmail.com',
                subject: "Demo Pipeline Build Pass",
                body:"Demo Build Done",
            to: 'yogewro@gmail.com',
            mimeType: 'text/html'
            }
        }
    failure {
            script {
                emailext from: 'trainwithshubham@gmail.com',
                subject: "Wanderlust Application build failed - '${currentBuild.result}'",
                body: "Build PhatGayi",
            to: 'yogewro@gmail.com',
            mimeType: 'text/html'
            }
        }
 }
}


