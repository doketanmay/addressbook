pipeline {
    agent any
    tools{
        maven 'mymaven'
    }
    environment{
        NEW_VERSION='1.4.0'
        APP_NAME='java-mvn-app'
    }
    
    stages {
        stage('COMPILE') {
            steps{
                script{
                    echo "Compiling the code"
    
                    sh 'mvn compile'
                }
            }
            
        }
        stage("UnitTest"){
            steps{
                script{
                    echo "Testing the code"
                    sh 'mvn test'
                }
            }
            post{
                always{
                    junit 'target/surefire-reports/*.xml'
                }
            }
            
        }
        stage("Package"){
            steps{
                script{
                    echo "Packing the code"
                    echo "Building new version ${NEW_VERSION}"
                    sh 'mvn package'
                }
            }
            
        }
        stage("BuildDockerImage"){
            
            steps{
                script{
                    echo "Building the image"
                    echo "Building ${NEW_VERSION}"
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                    sh 'sudo docker build -t doketanmay01/myprivaterepo:$BUILD_NUMBER .'
                    sh 'sudo docker login -u $USERNAME -p $PASSWORD'
                    sh 'sudo docker push doketanmay01/myprivaterepo:$BUILD_NUMBER'
                    
                    }

                    
                }
            }
            
        }
        stage("Deploy on EKS"){
            steps{
                script{
                    echo "Deploying the app"
                    sh 'envsubst < java-mvn-deploy-svc.yml | sudo /usr/local/bin/kubectl apply -f -'
                }
            }
        }
    }
}
