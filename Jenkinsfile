pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    // Checkout code from Git repository
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: "master"]],
                        userRemoteConfigs: [[url: 'https://github.com/saikiran1740/nodepoc.git']]
                    ])
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Install dependencies using npm
                    sh 'npm install'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run tests using Jest (or your test runner)
                    sh 'npm test'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build the NestJS project
                    sh 'npm run build'
                }
            }
        }

        stage('Deploy') {
            when {
                expression {
                    // Deploy only for specific branches (e.g., 'main' and 'staging')
                    return env.BRANCH_NAME == 'main' || env.BRANCH_NAME == 'master'
                }
            }
            steps {
                script {
                    // Add your deployment logic here
                    echo "Deploying application to production environment from branch ${env.BRANCH_NAME}..."
                    // Example deployment command
                    // sh 'scp -r ./dist user@server:/path/to/deploy'
                }
            }
        }
    }

    post {
        always {
            // Archive test results and build artifacts
            junit '**/test-results/**/*.xml'
            archiveArtifacts artifacts: 'dist/**/*', allowEmptyArchive: true
        }

        success {
            // Notify on success
            echo 'Build and deployment successful!'
        }

        failure {
            // Notify on failure
            echo 'Build failed. Please check the console output for details.'
        }
    }
}
