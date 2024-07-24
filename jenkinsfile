pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                script {
                    docker.build('flask_app:1.0.0', '.')
                }
            }
        }
        
        stage('Test') {
            steps {
                script {
                    docker.image('flask_app:1.0.0').withRun('-p 4000:4000 --name flask_app --link flask_db -e DB_URL=postgresql://postgres:postgres@flask_db:5432/postgres') {
                        // Perform testing steps here
                    }
                }
            }
        }
    }
    
    post {
        always {
            script {
                docker.image('flask_app:1.0.0').stop()
                docker.image('flask_app:1.0.0').remove()
                docker.image('postgres:12').stop()
                docker.image('postgres:12').remove()
                docker.volume('pgdata').remove()
            }
        }
    }
}
