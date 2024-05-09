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
                sh 'npm install -g heroku'
                script {
                    withCredentials([[$class: 'UsernamePasswordMultiBinding', usernameVariable: 'HEROKU_EMAIL', passwordVariable: 'HEROKU_API_KEY']]) {
                        sh 'heroku login -i'
                        sh 'heroku create'
                        sh 'git push heroku main'
                        sh 'heroku ps:scale web=1'
                    }
                }
            }
        }
    }
}
