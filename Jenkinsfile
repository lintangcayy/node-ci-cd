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
                bat 'npm run integration-test'
            }
        }
        stage('Build') {
            steps {
                echo 'Building the application...'
                // Proses build proyek
                bat 'npm run build'
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
                bat 'npm run deploy-staging'
            }
        }
    }
    post {
        success {
            // Mengirim notifikasi email jika build sukses
            emailext subject: "Build Succeeded on ${env.BRANCH_NAME}",
                     body: "Build for branch ${env.BRANCH_NAME} completed successfully.",
                     recipientProviders: [[$class: 'DevelopersRecipientProvider']]
        }
        failure {
            // Mengirim notifikasi email jika build gagal
            emailext subject: "Build Failed on ${env.BRANCH_NAME}",
                     body: "Build for branch ${env.BRANCH_NAME} failed. Please check the logs.",
                     recipientProviders: [[$class: 'DevelopersRecipientProvider']]
        }
        unstable {
            // Mengirim notifikasi email jika build dalam status unstable
            emailext subject: "Build Unstable on ${env.BRANCH_NAME}",
                     body: "Build for branch ${env.BRANCH_NAME} is unstable. Please review the test results.",
                     recipientProviders: [[$class: 'DevelopersRecipientProvider']]
        }
        always {
            // Membersihkan workspace setelah proses selesai
            cleanWs()
        }
    }
}
