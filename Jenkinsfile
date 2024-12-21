pipeline {
    agent any
    environment {
        CI = 'true' // Menandakan bahwa ini adalah proses CI/CD
    }
    stages {
        stage('Checkout') {
            steps {
                script {
                    // Mengambil nama branch dari environment variable atau default ke 'main'
                    def branchName = env.BRANCH_NAME ?: 'main'
                    echo "Checking out branch: ${branchName}"
                    // Mengambil kode dari branch yang relevan
                    git branch: branchName, url: 'https://github.com/lintangcayy/node-ci-cd.git'
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                // Menginstal semua dependensi proyek
                bat 'npm install'
            }
        }
        stage('Run Unit Tests') {
            steps {
                // Menjalankan unit test untuk memastikan aplikasi berfungsi dengan baik
                bat 'npm test'
            }
        }
        stage('Run Integration Tests') {
            when {
                // Pengujian integrasi hanya dilakukan pada branch 'main' dan 'develop'
                anyOf {
                    branch 'main'
                    branch 'develop'
                }
            }
            steps {
                echo 'Running integration tests...'
                // Menjalankan integrasi test
                bat 'npm run integration-test' // Tambahkan perintah untuk pengujian integrasi
            }
        }
        stage('Build') {
            steps {
                echo 'Building the application...'
                // Proses build proyek
                bat 'npm run build' // Proses build aplikasi
            }
        }
        stage('Deploy to Staging') {
            when {
                // Deployment hanya dilakukan di branch 'main'
                branch 'main'
            }
            steps {
                echo 'Deploying to staging server...'
                // Proses deployment ke server staging
                bat 'npm run deploy-staging' // Script untuk deploy ke server staging
            }
        }
    }
    post {
        success {
            // Mengirim notifikasi email jika build sukses
            emailext subject: 'Build Succeeded',
                     body: 'The build on branch ${env.BRANCH_NAME} succeeded!',
                     recipientProviders: [[$class: 'DevelopersRecipientProvider']]
        }
        failure {
            // Mengirim notifikasi email jika build gagal
            emailext subject: 'Build Failed',
                     body: 'The build on branch ${env.BRANCH_NAME} failed.',
                     recipientProviders: [[$class: 'DevelopersRecipientProvider']]
        }
    }
}
