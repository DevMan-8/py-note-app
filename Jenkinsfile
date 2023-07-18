pipeline {
    agent {label 'worknode'}
    stages{
        stage("cloning the code"){
            steps{
               echo "cloning the code" 
               git url:"https://github.com/DevMan-8/py-note-app.git", branch: "main"
            }
        }
        stage("build"){
            steps{
                echo "Building the images"
                sh "docker build -t my-note-app ."
            }
        }
        stage("push to Docker Hub"){
            steps{
               withCredentials([usernamePassword(credentialsId: "d-pass", passwordVariable: "d_passPass", usernameVariable: "d_passUser")]) {
                sh "docker tag my-note-app ${env.d_passUser}/my-note-app:latest"
                sh "docker login -u ${env.d_passUser} -p ${env.d_passPass}"
                sh "docker push ${env.d_passUser}/my-note-app:latest"
               }
            }
        }
       // stage("push to Docker Hub"){
         //   steps{
           //     sh "docker push ${env.d_passUser}/my-note-app:latest"
            //}
        //}
        stage(deploy){
            steps{
                sh '''
                docker kill python3 || true
                docker run -d --rm -p 443:8000 --name python3 checkmate123/my-note-app:latest
                '''
            }
        }
    }
}
