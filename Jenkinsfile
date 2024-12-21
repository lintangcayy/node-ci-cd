pipeline {
    agent any
    environment {
        CI = 'true' // Menandakan proses ini adalah bagian dari CI/CD
    }
    stages {
        stage('Checkout') {
            steps {
                // Mengambil kode dari branch yang relevan
                script {
                    def branchName = env.BRANCH_NAME ?: 'main'
                    git branch: branchName, url: 'https://github.com/lintangcayy/node-ci-cd.git'
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install' // Menginstal semua dependensi proyek
            }
        }
        stage('Run Unit Tests') {
            steps {
                sh 'npm test' // Menjalankan unit test
            }
        }
        stage('Run Integration Tests') {
            when {
                // Tahap ini hanya akan dijalankan di branch 'main'
                branch 'main'
            }
            steps {
                echo 'Running integration tests...'
                sh 'npm run integration-test' // Tambahkan perintah untuk pengujian integrasi
            }
        }
        stage('Build') {
            steps {
                echo 'Building the application...'
                sh 'npm run build' // Proses build proyek
            }
        }
        stage('Deploy to Staging') {
            when {
                // Deployment hanya dilakukan di branch 'main'
                branch 'main'
            }
            steps {
                echo 'Deploying to staging server...'
                sh 'npm run deploy-staging' // Script deployment ke server staging
            }
        }
    }
    post {
        success {
            emailext subject: 'Build Succeeded',
                     body: 'The build on branch ${env.BRANCH_NAME} succeeded!',
                     recipientProviders: [[$class: 'DevelopersRecipientProvider']]
        }
        failure {
            emailext subject: 'Build Failed',
                     body: 'The build on branch ${env.BRANCH_NAME} failed.',
                     recipientProviders: [[$class: 'DevelopersRecipientProvider']]
        }
    }
}
