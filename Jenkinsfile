pipeline {
    agent { label 'node-agent' }
    stages {
        stage('Code') {
            steps {
                echo 'Cloning source code...'
                git url: 'https://github.com/devopswithsky/node-todo-cicd.git' , branch: 'master'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application...'
                sh 'docker build -t dockercycle/node-todo-app:latest .'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
            }
        }
        stage('Push') {
            steps {
                echo 'Running Push...'
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerHub', 
                        passwordVariable:'dockerHubPassword',
                        usernameVariable:'dockerHubUser'
                        )
                    ]){
                    sh "docker login -u ${env.dockerHubUser}  -p ${env.dockerHubPassword}"
                    sh 'docker push dockercycle/node-todo-app:latest'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                sh "docker compose down && docker compose up -d"
 
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
        always {
            echo 'Pipeline execution finished.'
        }
    }
}

