pipeline {
    agent any
    environment {
        CI = 'true'
    }
    stages {
        stage('Checkout') {
            steps {
                // Ganti 'username' dengan 'lintangcayy'
                git branch: 'main', url: 'https://github.com/lintangcayy/node-ci-cd.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                // Menginstal semua dependensi proyek
                sh 'npm install'
            }
        }
        stage('Run Unit Tests') {
            steps {
                // Menjalankan unit test
                sh 'npm test'
            }
        }
        stage('Build') {
            steps {
                echo 'Building the application...'
                // Tambahkan perintah build jika proyek memiliki proses build
                // Contoh: sh 'npm run build'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
                // Tambahkan perintah deploy jika proyek memiliki proses deploy
                // Contoh: sh 'npm run deploy'
            }
        }
    }
    post {
         success {
             emailext subject: 'Build Succeeded', body: 'The build succeeded!', recipientProviders: [[$class: 'DevelopersRecipientProvider']]
        }
         failure {
             emailext subject: 'Build Failed', body: 'The build failed.', 
recipientProviders: [[$class: 'DevelopersRecipientProvider']]
        }
    }
}
