pipeline {
    agent any
     tools {
        nodejs 'nodejs'
    }
    stages {
        stage('checkout') {
            steps {
                git url: 'https://github.com/akshu20791/react-project-businesswebsite/'
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'
                // sh 'npm run build'
            }
        }
        stage('Test') {
            steps {
                // sh 'npm run test'
                echo "Test"
            }
        }
        stage('Build Image') {
            steps {
                sh 'docker build -t businesswebsite .'
                sh 'docker tag businesswebsite:latest akshu20791/businesswebsite:latest'
            }
        }
        stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockercred', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push akshu20791/businesswebsite:latest'
                }
            }
        }
         stage('Deploy') {
            steps {
               script {
                    def dockerCmd = 'sudo docker run -itd --name My-first-containe211 -p 8090:80 akshu20791/businesswebsite:latest'
                    sshagent(['sshkeypair']) {
                        //chnage the private ip in below code
                        // sh "docker run -itd --name My-first-containe211 -p 8082:80 akshu20791/phpproject:v1"
                         sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.83.223 ${dockerCmd}"
                    }
                }
            }
        }
    }
}
