pipeline {
    agent {
        docker {
            image 'node:16-buster-slim' 
            args '-p 3000:3000' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Manual Approval') {
            steps {
                def userInput = input message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed', parameters: []
                if (!userInput) {
                    // Jika tombol "Abort" ditekan, jalankan kill.sh
                    sh './jenkins/scripts/kill.sh'
                }
                // input message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed'
                // sh './jenkins/scripts/kill.sh' 
            }
        }
        stage('Deploy') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
                sleep(time: 1, unit: 'MINUTES')
                // input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)' 
                sh './jenkins/scripts/kill.sh' 
            }
        }
    }
}
