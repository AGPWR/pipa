pipeline {
    agent any

    tools {
        nodejs "node22.1.0"
    }

    stages {
        stage('Install') {
            steps {
                sh 'npm ci'
            }
        }

        stage('Build') {
            steps {
                sh 'cd server'
                sh 'npm start &'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Deploy to Heroku') {
            steps {
                sh 'su -c "curl https://cli-assets.heroku.com/install.sh | sh" admin'
                script {
                    withCredentials([[$class: 'StringBinding', credentialsId: 'HEROKU_API_KEY', variable: 'HEROKU_API_KEY']]) {
                        sh 'heroku login --no-interactive'
                        sh 'heroku create'
                        sh 'git push heroku main'
                        sh 'heroku ps:scale web=1'
                    }
                }
            }
        }
    }
}
