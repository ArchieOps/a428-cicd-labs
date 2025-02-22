//Test cronjob build

node {
    stage('Checkout') {
        // Tambahkan stage checkout
        checkout scm
    }
    docker.image('node:16-buster-slim').inside('-p 3000:3000') {
        stage('Build') {
                sh 'npm install'
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
                sh './jenkins/scripts/deliver.sh' 
                input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)' 
                sh './jenkins/scripts/kill.sh' 
                // sh 'apt-get update && apt-get -y install openssh-client'
                // sh 'scp -o StrictHostKeyChecking=no -i $privateKey target/*.jar ubuntu@ec2-18-139-95-243.ap-southeast-1.compute.amazonaws.com:/home/ubuntu/simple-java-maven-app'
                // sh 'ssh -o StrictHostKeyChecking=no -i $privateKey ubuntu@ec2-18-139-95-243.ap-southeast-1.compute.amazonaws.com java -jar /home/ubuntu/simple-java-maven-app/*.jar'
                sleep (time: 60, unit: 'SECONDS');
                echo 'Deployed'
            }
        }
    }
}