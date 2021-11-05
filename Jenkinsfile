pipeline {
    agent any
    tools{
        maven 'mymaven'
    }
    environment{
        NEW_VERSION='1.4.0'
    }
    
    stages {
        stage('COMPILE') {
            steps{
                script{
                    echo "Compiling the code"
                    git 'https://github.com/doketanmay/addressbook.git'
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
    }
}
