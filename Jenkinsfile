//Test cronjob build

node {
    stage('Checkout') {
        // Tambahkan stage checkout
        checkout scm
    }
    docker.image('node:16-buster-slim').inside('-p 3000:3000 -u root') {
        stage('Build') {
                sh 'pwd'
                // sh 'ls -la'
                sh 'npm install'
                // sh 'ls -la'
            }
        stage('Test') { 
                sh './jenkins/scripts/test.sh' 
        }
        stage('Manual Approval'){
            input message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed'
        }
        stage("deploy"){
            sh 'echo "Deploying to server"'
            withCredentials([sshUserPrivateKey(credentialsId: 'private-key-aws-java-app', keyFileVariable: 'privateKey')]) {
                sh 'apt-get update && apt-get -y install openssh-client'
                sh 'scp -o StrictHostKeyChecking=no -i $privateKey -r build ubuntu@ec2-52-76-233-190.ap-southeast-1.compute.amazonaws.com:/home/ubuntu/react-app'
                // sh 'ssh -o StrictHostKeyChecking=no -i $privateKey ubuntu@ec2-52-76-233-190.ap-southeast-1.compute.amazonaws.com java -jar /home/ubuntu/react-app'
                sleep (time: 60, unit: 'SECONDS');
                echo 'Deployed'
            }
        }
    }
}