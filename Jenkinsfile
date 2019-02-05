pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Images'){
            
            steps{
                script {
                    app = docker.build("satadal7/node-app")
                    app.inside {
                        sh 'echo $(curl localhost:8080)'
                    }
                }
            }
        }
        stage('push Docker Image'){
           
            steps{
                script{
                    docker.withRegistry('https://registry.hub.docker.com','docker_hub_login'){
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }             
                      
    }
}
