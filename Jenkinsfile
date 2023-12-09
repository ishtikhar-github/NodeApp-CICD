pipeline{
    agent any
    stages{
        stage("Code Clone"){
            steps{
                git url: "https://github.com/ishtikhar-github/NodeApp-CICD.git", branch: "master"
                echo "code clone ho gaya"
            }
        }
        stage("Code Copy & Build"){
            steps{
             sh "sudo cp -r . /home/ubuntu/nodeapp"
             sh "docker build -t nodeapp-test ."
             echo " Code Copied and Build"
            }
        }
        stage("Code Push"){
            steps{
                    withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerpass",usernameVariable:"dockeruser")]){
                    sh "docker login -u ${env.dockeruser} -p ${env.dockerpass}"
                    sh "docker tag nodeapp-test:latest ${env.dockeruser}/nodeapp-test:latest"
                    sh "docker push ${env.dockeruser}/nodeapp-test:latest"
                    echo "image pushed done"
                
                }
            }
        }
         stage("deploy"){
            steps{
                sh '''
                sudo docker-compose -f /home/ubuntu/nodeapp/docker-compose.yaml down
                sudo docker-compose -f /home/ubuntu/nodeapp/docker-compose.yaml up -d --build
                '''
                echo 'deployment done'
            }
        }
    }
     post { 
        success { 
            echo 'your pipeline executed successfully'
        }
    }
}
