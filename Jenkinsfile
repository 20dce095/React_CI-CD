pipeline {
    agent any
    environment{
        IMAGE_NAME= 'pradippipaliya/react_app:1.1'
    }

    stages {
        stage("init") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }
        stage("Build app") {
            steps {
                script {
                    echo "building the react application"
                   
                }
            }
        }
        stage("Build Image") {
            steps {
                script {
                    echo "building the docker image and push to docker hub repositroy"
                    //gv.buildImage()
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-pradippipaliya', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]){
                        sh 'docker build -t ${IMAGE_NAME} .'
                        sh 'docker login -u $USERNAME -p $PASSWORD'
                        sh 'docker push ${IMAGE_NAME}'
                    }
                }
            }
        }
        stage("Deploy Application") {
            steps {
                script {
                    echo "deploying the spring application on ec2,,,"
                    //gv.deployApp()
                    def dockerStop="docker stop ec2-react"
                    def dockerDelete="docker rm ec2-react"
                    def dockerCreate="docker run -p 3000:3000 --name ec2-react ${IMAGE_NAME}"

                    sshagent(['ec2-linux-key']) {
                        
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@35.175.246.184  ${dockerCreate}"

                    }
                }
            }
        }
    }
}
