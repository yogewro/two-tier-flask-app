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
                    
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
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
        post{
            sucess{
                script{
                    emailtext from : "yogewro@gmail.com",
                        subject :"demo build has been done-5G layer UP",
                        body:"Build Done",
                    to:"yogewroclaw@gmail.com",
                    mimeType: 'test/html'                          
                                           }
            }
            failure{
                script{
                    emailtext from : "yogewro@gmail.com",
                        subject :"demo build Failed",
                        body:"Build failed",
                    to:"yogewroclaw@gmail.com",
                    mimeType: 'test/html'
           
                }
            }
        }
        
    }
}
