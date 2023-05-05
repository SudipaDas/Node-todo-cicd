pipeline {
    agent { label 'Production-agent' }
    
    stages{
        stage('Code'){
            steps {
                git url: 'https://github.com/SudipaDas/Node-todo-cicd.git', branch: 'main'
            }
        }
        stage('Build and Test'){
            steps {
                sh 'docker build . -t sudipadas/node-todo-cicd:latest' 
            }
        }
        stage('Loging Dockerhub and push image'){
            steps {
                echo 'login and push'
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerhubpassword', usernameVariable: 'dockerhubuser')]){
                    //sh 'echo dockerhubuser'
                    // also available as a Groovy variable
                    //echo dockerhubuser
                    // or inside double quotes for string interpolation
                    echo "username is $dockerhubuser"
                    sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpassword}"
                    sh "docker push $dockerhubuser/node-todo-cicd:latest"
                }
            }
        }
        stage('Deploy'){
            steps {
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }
}
