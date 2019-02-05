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
            when {
                branch 'master'
            }
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
            when {
                branch 'master'
            }
            steps{
                script{
                    docker.withRegistry('https://registry.hub.docker.com','satadal7'){
                        app.push("$(env.BUILD_NUMBER)")
                        app.push("latest")
                    }
                }
            }
        }             
                      
    }
}
