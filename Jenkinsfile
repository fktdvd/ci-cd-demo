pipeline {
    agent any

    stages {
        stage('Clone repo') {
            steps {
                echo 'Repo a Jenkins által kerül klónozásra SCM-ből.'
            }
        }

        stage('Build Docker image') {
            steps {
                sh 'docker build -t ci-demo .'        
            }
        }

        stage('Run tests in Docker') {
            steps {
                sh 'docker run ci-demo'                
            }
        }
    }
}